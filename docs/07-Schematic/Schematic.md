---
title: Temperature Subsystem Schematic
---

## Overview

This schematic represents the final design of the Environmental Sensor Subsystem Board. The system converts a 9V DC wall adapter input into a regulated 3.3V supply using an LM2575T-3.3G switching regulator with input protection fuse and diode. The regulated rail powers a PIC18F57Q43 microcontroller and a TC74 digital temperature sensor.

The microcontroller processes temperature data from the sensor and communicates with other subsystem boards through a ribbon connector interface. The design includes programming (ICSP), debug interfaces, and expansion I/O to support system integration and testing.

Key design priorities include:
- Stable and efficient 3.3V power regulation
- Reliable I2C temperature sensing (TC74)
- Robust inter-board communication via ribbon connectors
- Safe power input protection fuse and reverse polarity protection
- Ease of programming and debugging

## Updated Schematic Diagram

<img width="3508" height="2480" alt="314 Sensor Subsystem" src="https://github.com/user-attachments/assets/427accc6-0d7e-4a1f-b703-4977c8e13015" />

## Updated Design Notes (Final System Changes)

### 1. Power System Update
- Final regulator selected: LM2575T-3.3G (switching regulator)
- Input: 9V DC wall adapter
- Output: 3.3V regulated rail
- Added protection:
  - Input fuse for overcurrent protection
  - diode for reverse polarity protection
- Switching regulator chosen for improved efficiency over linear alternatives under sensor + MCU load conditions

### 2. Sensor Update
- Final sensor selection: TC74 digital temperature sensor
- Interface: I2C (2-wire digital communication)
- Replaced earlier multi-sensor approach with a single dedicated temperature sensor for:
  - Simpler firmware
  - Reduced pin usage
  - Improved reliability

### 3. Microcontroller Integration
- MCU: PIC18F57Q43 (48-pin)
- Responsibilities:
  - Reads temperature data via I2C
  - Handles UART communication to other boards
  - Controls GPIO-based debug features
  - Supports ICSP programming interface

### 4. System Communication
- External communication via 2×4 ribbon connectors:
  - 9V upstream and downstream
  - Common ground
  - UART TX/RX
  - Digital I/O expansion lines if needed
  - Inter-board subsystem communication bus

### 5. Design Improvements From Initial Version
- Replaced earlier sensor concept with TC74-only temperature measurement
- Confirmed switching regulator as final power solution
- Reduced system complexity by removing redundant sensor pathways
- added diode for reverse flow issues
- added pull down resistors to not have floating pins
- Standardized all subsystem communication through ribbon connectors
- Replaced some capacitors with better capacity.

## Deliverables

- High-resolution PDF schematic: [Schematic PDF](https://github.com/user-attachments/files/27211462/314.Sensor.Subsystem.pdf)
- ECAD project ZIP file: [ECAD Project ZIP](https://github.com/user-attachments/files/27212208/314.Temperature.Sensor.Subsystem.zip)
- Too see the PCB Design page click here: [PCB Design](https://isaiahelixir1.github.io/isaiahelixer1.github.io/08-PCB-Design/PCB%20Design/)



