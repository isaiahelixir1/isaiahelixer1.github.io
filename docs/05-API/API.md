# Environmental Subsystem API

## Overview
This subsystem is the environmental sensor system in a 7-node UART daisy-chain network.  
It interfaces with a TC74 environmental temperature sensor using I2C and communicates with the UART daisy chain.

The subsystem is responsible for:
- Reading temperature data from the TC74 sensor  
- Sending formatted UART messages (periodic status or on-demand responses)  
- Receiving, processing, and forwarding UART messages in the daisy chain  
- Responding to temperature requests message type 0x08  
- Sending ACKs for messages addressed to it  
- Discarding invalid/loopback/unapproved messages  

## System Architecture
7 subsystems connected in a UART daisy chain 9600 baud rate. Each subsystem:
- Receives messages via UART1  
- Processes messages addressed to itself MY_ID = 0x49  
- Forwards all other messages immediately and unchanged if target is approved or broadcast  
- Discards messages from self loopback protection  
- Discards messages from unapproved senders  
- Prioritizes forwarding over new transmissions  

Approved node IDs:
0x43 – Christo  
0x4C – Liam    
0x49 – Isaiah  
0x52 – Ragul  
0x58 – BCAST  

## Sensor Interface (I2C)
TC74 temperature sensor accessed via I2C1 at address 0x4C.  

Temperature read:
- Write register 0x0  
- Read 1 byte (signed 8-bit value = °C)  
- No calibration registers needed  

I2C details:
- Slave address: 0x4C  
- Supports simple write-then-read  
- Error recovery routine present i2c_recover()

## Packet Format (Current Implementation)
All messages use this exact 7 + data_len byte structure:

| Byte(s)              | Content                  | Type      | Value / Notes                          |
|----------------------|--------------------------|-----------|----------------------------------------|
| 0–1                  | Header                   | uint8_t   | 0x41 0x5A (“AZ”)                    |
| 2                    | Sender ID                | uint8_t   | MY_ID or other approved ID           |
| 3                    | Target ID                | uint8_t   | MY_ID, BCAST, or other             |
| 4                    | Message Type mtype   | uint8_t   | See table below                        |
| 5 … (5+data_len-1)   | Data                     | uint8_t[] | Variable length payload                |
| (5+data_len)         | Footer byte 1            | uint8_t   | 0x59 (“Y”)                           |
| (6+data_len)         | Footer byte 2            | uint8_t   | 0x42 (“B”)                           |

- Maximum packet size: 128 bytes MAX_PKT  
- Packet is built with build_packet() and sent with send_pkt()  

## Message Types Used (Current)

| mtype (hex) | Description                          | Data Length | Data Content (current)                  |
|-------------|--------------------------------------|-------------|-----------------------------------------|
| 0x08      | Temperature Data / Request           | 1 byte      | Signed temperature (°C)                 |
| 0x4B      | Acknowledgement (ACK)                | 1 byte      | Original mtype that was received      |
| 0x10      | Test / Periodic message (default)    | 1 byte      | Current temperature byte                |

## Incoming Message Handling (`handle_packet`)
1. Validate prefix 0x41 0x5A and suffix 0x59 0x42.
2. Discard if sender == MY_ID (loopback).
3. Discard if sender not in approved list.
4. If target == MY_ID:
   - Send ACK (0x4B).
   - If mtype == 0x08: read TC74 and reply immediately with 0x08 + temperature byte.
5. **If target == BCAST or approved node**:
   - Forward packet unchanged.
6. Otherwise: drop.

## Forwarding Behavior
- Non-local messages are forwarded immediately (prioritized).
- No modification of forwarded packets.

## Sender Behavior
- Periodic transmission every ~500 ms TX_RATE_MS.
- Uses non-blocking cooldown counter.
- Currently sends type 0x10 to test_target (default = BCAST) with 1-byte temperature.
- Change test_target, test_mtype, or data length in send_my_own_message() as needed.
- Transmission LED (RB0) blinks on every TX.

## Additional Features in Code
- RX state machine WAIT_P1 / WAIT_P2 / COLLECT for reliable packet assembly.
- Debug output over UART (sender name, raw hex, ASCII data).
- RB0 LED blinks on every RX or TX.
- I2C timeout + recovery.
- Software reset on RB2 high.
- Interrupt enabled U1RXIE but RX is currently polled in main loop.
- Ring buffer declared but unused.

## Notes / Future Alignment
- The ring buffer ring_buf is declared but not currently used.
- To align with the original BME680 plan:
  - Replace tc74_read() with full BME680 forced-mode read + compensation.
  - Expand periodic messages to types 0x08/0x10/0x11 with scaled multi-byte payloads.
  - Add oversampling/IIR configuration at boot.
