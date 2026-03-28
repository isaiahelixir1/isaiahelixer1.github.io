# Subsystem API

## Overview
This subsystem interfaces with a BME680 environmental sensor using I2C and communicates with a 7-node daisy-chain UART network.

The subsystem is responsible for:  
- Reading environmental data (temperature, humidity, pressure, gas resistance)  
- Processing sensor data using calibration registers  
- Sending formatted UART messages to other subsystems  
- Receiving, processing, and forwarding UART messages in the daisy chain  

**Note:** This subsystem **does not generate unsolicited signals**; it only transmits sensor data or acknowledgements for messages addressed to it.

## System Architecture
7 subsystems connected in a UART daisy chain. Each subsystem:  
- Receives messages  
- Processes messages addressed to it  
- Forwards all other messages **immediately and unchanged**  
- Discards duplicate messages to prevent retransmission loops  

## Sensor Interface (I2C)
The BME680 is accessed via I2C.

### Temperature Registers
| Variable | Register Address |
|----------|----------------|
| par_t1   | 0xE9 / 0xEA    |
| par_t2   | 0x8A / 0x8B    |
| par_t3   | 0x8C           |
| temp_adc | 0x24<7:4> / 0x23 / 0x22 |

### Pressure Registers
| Variable | Register Address |
|----------|----------------|
| par_p1   | 0x8E / 0x8F    |
| par_p2   | 0x90 / 0x91    |
| par_p3   | 0x92           |
| par_p4   | 0x94 / 0x95    |
| par_p5   | 0x96 / 0x97    |
| par_p6   | 0x99           |
| par_p7   | 0x98           |
| par_p8   | 0x9C / 0x9D    |
| par_p9   | 0x9E / 0x9F    |
| par_p10  | 0xA0           |
| press_adc | 0x21<7:4> / 0x20 / 0x1F |

### Humidity Registers
| Variable | Register Address |
|----------|----------------|
| par_h1   | 0xE2<3:0> / 0xE3 |
| par_h2   | 0xE2<7:4> / 0xE1 |
| par_h3   | 0xE4            |
| par_h4   | 0xE5            |
| par_h5   | 0xE6            |
| par_h6   | 0xE7            |
| par_h7   | 0xE8            |
| hum_adc  | 0x26 / 0x25    |

### Gas Registers
| Variable  | Register Address |
|-----------|----------------|
| gas_adc   | 0x2B<7:6> / 0x2A |
| gas_range | 0x2B<3:0>       |

## Message Types Used
| Message Type | Description |
|--------------|-------------|
| 8            | Temperature Sensor Data Report |
| 10           | Barometric Pressure Data Report |
| 11           | Humidity Sensor Data Report |
| 13           | System Status Report |
| 14           | System Error Code Report |
| 16           | Heartbeat |

### Message Definitions

**Message Type 8 — Temperature Sensor Data Report**

| Byte | Variable Name | Type    | Min   | Max   |
|------|---------------|--------|-------|-------|
| 1–2  | message_type  | uint16_t | 8     | 8     |
| 3–4  | temperature   | int16_t  | -4000 | 8500  |

*Temperature is scaled by 100 (°C × 100)*

**Message Type 10 — Pressure Data Report**

| Byte | Variable Name | Type     | Min    | Max     |
|------|---------------|---------|--------|---------|
| 1–2  | message_type  | uint16_t | 10    | 10      |
| 3–6  | pressure      | uint32_t | 30000 | 110000 |
| 7–10 | altitude      | int32_t  | -50000 | 100000 |

**Message Type 11 — Humidity Data Report**

| Byte | Variable Name | Type    | Min   | Max   |
|------|---------------|--------|-------|-------|
| 1–2  | message_type  | uint16_t | 11    | 11    |
| 3–4  | humidity      | uint16_t | 0     | 10000 |

*Humidity is scaled by 100 (% × 100)*

## Sensor Configuration
- **Operating Mode:** Forced Mode (trigger measurements on demand)  
- **Oversampling:** Improves measurement resolution (×1 to ×16 for T/P/H)  
- **IIR Filter:** Reduces temperature and pressure noise (coefficients 0–127)  
- **Measurement Status Flags:**  
  - new_data → new measurement available  
  - measuring → measurement in progress  
  - gas_measuring → gas measurement in progress  

## I2C Communication Details
- Slave address: 0x76 or 0x77 (depends on SDO pin)  
- Supports Start, Stop, repeated start, auto-increment reads  
- Soft reset: 0xB6 to 0xE0  

## Message Processing Logic

### Incoming Message Handling
1. Check message validity: proper frame, correct length, valid type  
2. Check sender: discard if from this subsystem  
3. Check destination:  
   - If addressed to this subsystem → process & send ACK  
   - Else → forward unchanged  
4. **Duplicate messages:** discard to prevent loops  

### Forwarding Behavior
- Immediate forwarding for all non-local messages  
- Prioritized over new transmissions  

### Acknowledgement Message
| Byte | Variable Name  | Type     | Description          |
|------|----------------|---------|--------------------|
| 1–2  | message_type   | uint16_t | Type of received message |
| 3    | received_msg   | uint8_t  | Original message ID       |
| 4    | status         | uint8_t  | 1 → success, 0 → error   |

## Sender Behavior
- Periodically transmit sensor readings: temperature, pressure, humidity  
- Messages must be formatted, payload ≤ limits, dynamically updated  
- Transmission controlled by a **non-blocking timer**  
- Data packed as **scaled integers** for reliable UART transmission  
