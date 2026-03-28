---
title: Subsystem API
tags:
- API
- BME680 Sensor
---

## Overview
This subsystem interfaces with a BME680 environmental sensor using I2C and communicates with a 7-node daisy-chain UART network.

The subsystem is responsible for:
- Reading environmental data (temperature, humidity, pressure, gas resistance)
- Processing sensor data using calibration registers
- Sending formatted UART messages to other subsystems
- Receiving, processing, and forwarding UART messages in the daisy chain

## System Architecture

- 7 subsystems connected in a UART daisy chain
- Each subsystem:
  - Receives messages
  - Processes messages addressed to it
  - Forwards all other messages

## Sensor Interface (I2C)

The BME680 is accessed via I2C.

### Temperature Registers
| Variable | Register Address |
|----------|-----------------|
| par_t1   | 0xE9 / 0xEA     |
| par_t2   | 0x8A / 0x8B     |
| par_t3   | 0x8C            |
| temp_adc | 0x24<7:4> / 0x23 / 0x22 |

### Pressure Registers
| Variable | Register Address |
|----------|-----------------|
| par_p1   | 0x8E / 0x8F     |
| par_p2   | 0x90 / 0x91     |
| par_p3   | 0x92            |
| par_p4   | 0x94 / 0x95     |
| par_p5   | 0x96 / 0x97     |
| par_p6   | 0x99            |
| par_p7   | 0x98            |
| par_p8   | 0x9C / 0x9D     |
| par_p9   | 0x9E / 0x9F     |
| par_p10  | 0xA0            |
| press_adc| 0x21<7:4> / 0x20 / 0x1F |

### Humidity Registers
| Variable | Register Address |
|----------|-----------------|
| par_h1   | 0xE2<3:0> / 0xE3 |
| par_h2   | 0xE2<7:4> / 0xE1 |
| par_h3   | 0xE4            |
| par_h4   | 0xE5            |
| par_h5   | 0xE6            |
| par_h6   | 0xE7            |
| par_h7   | 0xE8            |
| hum_adc  | 0x26 / 0x25     |

### Gas Registers
| Variable | Register Address |
|----------|-----------------|
| gas_adc  | 0x2B<7:6> / 0x2A |
| gas_range| 0x2B<3:0>       |

## Message Types Used

| Message Type | Description |
|-------------|------------|
| 8           | Temperature Sensor Data Report |
| 10          | Barometric Pressure Data Report |
| 11          | Humidity Sensor Data Report |
| 13          | System Status Report |
| 14          | System Error Code Report |
| 16          | Heartbeat |

## Message Definitions

### Message Type 8 — Temperature Sensor Data Report

| Byte | Variable Name | Type     | Min   | Max   |
|------|--------------|----------|-------|-------|
| 1–2  | message_type | uint16_t | 8     | 8     |
| 3–4  | temperature  | int16_t  | -4000 | 8500  |

Temperature is scaled by 100 (°C × 100)

### Message Type 10 — Pressure Data Report

| Byte | Variable Name | Type     | Min    | Max     |
|------|--------------|----------|--------|---------|
| 1–2  | message_type | uint16_t | 10     | 10      |
| 3–6  | pressure     | uint32_t | 30000  | 110000  |
| 7–10 | altitude     | int32_t  | -50000 | 100000  |

### Message Type 11 — Humidity Data Report

| Byte | Variable Name | Type     | Min | Max   |
|------|--------------|----------|-----|-------|
| 1–2  | message_type | uint16_t | 11  | 11    |
| 3–4  | humidity     | uint16_t | 0   | 10000 |

Humidity is scaled by 100 (% × 100)

## Sensor Configuration

The BME680 sensor is configured via I2C control registers to ensure accurate and stable measurements.

### Operating Mode

| Mode Bits | Mode        |
|----------|------------|
| 00       | Sleep Mode |
| 01       | Forced Mode |

The subsystem uses **Forced Mode** to trigger measurements on demand.

### Oversampling Settings

Oversampling improves measurement resolution:

#### Temperature (osrs_t)
| Setting | Oversampling |
|--------|-------------|
| 001    | ×1 |
| 010    | ×2 |
| 011    | ×4 |
| 100    | ×8 |
| 101+   | ×16 |

#### Pressure (osrs_p)
| Setting | Oversampling |
|--------|-------------|
| 001    | ×1 |
| 010    | ×2 |
| 011    | ×4 |
| 100    | ×8 |
| 101+   | ×16 |

#### Humidity (osrs_h)
| Setting | Oversampling |
|--------|-------------|
| 001    | ×1 |
| 010    | ×2 |
| 011    | ×4 |
| 100    | ×8 |
| 101+   | ×16 |

### IIR Filter

| Setting | Coefficient |
|--------|------------|
| 000    | 0 |
| 001    | 1 |
| 010    | 3 |
| 011    | 7 |
| 100    | 15 |
| 101    | 31 |
| 110    | 63 |
| 111    | 127 |

The filter is applied to temperature and pressure data to reduce noise.

### Measurement Status Flags

| Flag | Description |
|------|------------|
| new_data | Indicates new measurement available |
| measuring | Sensor is currently performing measurement |
| gas_measuring | Gas measurement in progress |

### I2C Communication Details

- Slave address: `0x76` or `0x77` (depending on SDO pin)
- Communication uses:
  - Start condition
  - Register address write
  - Repeated start for read
  - Auto-increment register reads

### Reset Behavior

- Writing `0xB6` to register `0xE0` triggers a soft reset

## Message Processing Logic

### Incoming Message Handling

Upon receiving a message:

1. **Check message validity**
   - Proper frame (prefix/suffix handled externally)
   - Correct length
   - Valid message type

2. **Check sender**
   - If message originated from this subsystem → discard

3. **Check destination**
   - If message is for this subsystem:
     - Process message
     - Send acknowledgement
   - Else:
     - Forward message unchanged

### Forwarding Behavior (Critical Requirement)

- All non-local messages are retransmitted
- Forwarding occurs immediately after reception
- Forwarding is prioritized over sending new messages

### Error Handling

The subsystem ignores:
- Malformed messages
- Messages exceeding buffer size
- Invalid message types
- Data outside expected ranges

## Acknowledgement Message

For every valid message received:

| Byte | Variable Name | Type     |
|------|--------------|----------|
| 1–2  | message_type | uint16_t |
| 3    | received_msg | uint8_t  |
| 4    | status       | uint8_t  |

- status = 1 → success  
- status = 0 → error  

## Sender Behavior

The subsystem periodically transmits:

- Temperature data
- Pressure data
- Humidity data

### Transmission Rules

- Messages are properly formatted
- Payload size does not exceed limits
- No reserved bytes appear in payload
- Data is dynamically updated
- Transmission rate is controlled via timer (non-blocking)

## Data Representation

To ensure reliable UART communication:

- Floating-point values are converted to scaled integers
- All values stay within defined data type ranges
- Data is packed efficiently into byte arrays

## System Integration Notes

- Message payload occupies bytes 4–61 of full UART packet
- Prefix, suffix, sender, and receiver handled by system protocol
- Subsystem uses I2C for sensor communication only
- UART used exclusively for inter-subsystem communication
