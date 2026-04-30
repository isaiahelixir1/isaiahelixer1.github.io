---
title: Environmental Subsystem API
---

## Overview
This subsystem is the temperature sensor node in a 7-node UART daisy-chain network.  
It interfaces with a TC74 temperature sensor using I2C and communicates using a fixed-format UART protocol.

The subsystem is responsible for:
- Reading temperature data from the TC74 sensor  
- Sending formatted UART messages using a fixed 64-byte packet structure  
- Receiving, processing, and forwarding UART messages in the daisy chain  
- Responding to temperature requests (MSG_REQUEST / MSG_TEMP)  
- Sending ACKs for valid received messages  
- Discarding invalid, loopback, or unapproved messages
  
## System Architecture
7 subsystems connected in a UART daisy chain at 9600 baud.

Each subsystem:
- Receives messages via UART1  
- Processes messages addressed to itself (MY_ID = 0x49)  
- Forwards all other valid messages unchanged  
- Discards loopback messages sender == self  
- Discards messages from unapproved nodes  
- Prioritizes forwarding over generation of new messages  

### Approved Node IDs
- 0x43 – Christo  
- 0x4C – Liam  
- 0x49 – Isaiah (this node)  
- 0x52 – Ragul  
- 0x41 – Arianna  
- 0x4D – Myles  
- 0x44 – Damian  
- 0x58 – Broadcast  

## Sensor Interface (I2C)
TC74 temperature sensor accessed via I2C1.

### Address
- 0x4C

### Read Procedure
- Write register 0x00
- Read 1 byte (signed 8-bit temperature in °C)
- Convert internally to scaled value:
  - temperature × 100

### Notes
- No calibration required  
- I2C recovery routine included (i2c_recover())  
- Invalid reads are discarded (0xFF check)  

## Packet Format (Fixed 64-byte Frame)

| Byte(s)              | Content        | Description |
|----------------------|----------------|-------------|
| 0–1                  | Prefix         | 0x41 0x5A ("AZ") |
| 2                    | Sender ID      | Node source |
| 3                    | Target ID      | Destination node |
| 4                    | Message Type   | Defines message behavior |
| 5–61                 | Payload        | Up to 57 bytes |
| 62                   | Footer 1       | 0x59 ("Y") |
| 63                   | Footer 2       | 0x42 ("B") |

- Fixed packet size: 64 bytes
- Built using build_packet()
- Transmitted using send_pkt()

## Message Types (Current Implementation)

| mtype (hex) | Description              | Purpose |
|-------------|--------------------------|---------|
| 0x08      | MSG_TEMP                | Temperature data / direct request |
| 0x14      | MSG_REQUEST             | Request specific data type |
| 0x4B      | MSG_ACK                 | Acknowledgement |

## Message Behavior

### MSG_TEMP (0x08)
- Contains temperature data or direct request response  
- Payload:
  - Byte 0–1: temperature ×100 (LSB first)
- Used when:
  - Responding to request
  - Direct temperature query

### MSG_REQUEST (0x14)
- Used to request data from a node  
- Payload:
  - Byte 0: requested message type

Example:
- payload[0] = 0x08 = request temperature

Behavior:
- If request is temperature:
  - TC74 is read
  - Response is sent using MSG_TEMP

### MSG_ACK (0x4B)
- Sent automatically after receiving valid messages (except ACK itself)
- Payload:
  - Byte 0: original message type being acknowledged

## Incoming Message Handling

1. Validate packet prefix (0x41 0x5A) and suffix (0x59 0x42)
2. Discard if sender == MY_ID (loopback protection)
3. Discard if sender is not in approved list
4. If receiver == MY_ID:
   - Send ACK (unless message is already ACK)
   - Handle message type:
     - MSG_REQUEST (0x14) = if payload[0] == 0x08 = send temperature
     - MSG_TEMP (0x08) = treat as direct request = send temperature
     - MSG_ACK (0x4B) = log acknowledgement
5. If receiver == BROADCAST:
   - Forward packet unchanged
6. If receiver is another approved node:
   - Forward packet unchanged
7. Otherwise:
   - Drop packet

## Forwarding Behavior
- Non-local messages are forwarded immediately
- Packets are never modified during forwarding
- Forwarding has priority over transmission

## Transmission Behavior
- No periodic transmissions
- Only transmits:
  - In response to requests
  - When sending ACKs
- LED (RB0) blinks on every TX and RX event

## Additional Features
- RX state machine: WAIT_P1 → WAIT_P2 → COLLECT
- Fixed 64-byte packet buffering
- UART debug output (hex + readable logs)
- I2C timeout + recovery system
- Software reset via RB2 input
- Interrupt-enabled UART RX (buffered via ring buffer)
