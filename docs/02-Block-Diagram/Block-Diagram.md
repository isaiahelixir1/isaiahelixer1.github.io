---
title: Individual Block Diagram
tags:
- tag1
- tag2
---

## Overview

This block diagram documents the layout and connections of my individual environmental monitoring subsystem. Its purpose is to clearly show how power, sensors, and communication interfaces are organized within the subsystem, and how this subsystem connects to the rest of the team’s boards.  

## Individual Block Diagram — Environmental Sensor Subsystem

<img width="881" height="642" alt="314 Sensor Subsystem" src="https://github.com/user-attachments/assets/4dbc19fa-c179-4c23-8398-1c63a2e5f85d" />

## Design Decision Process & Requirements Alignment

The primary goals of this subsystem were to measure environmental temperature, communicate with external systems, integrate with team subsystems, support debugging, and maintain reliable power operation.

The PIC18F57Q43 microcontroller was selected as the central component because it integrates multiple peripherals (I²C, UART, PWM, and GPIO). This microcontroller helped reduced the need for external components and was cost effective for the overall hardware design.

The TC74 temperature sensor was chosen due to its simple I²C communication interface, which provided reliable digital data transfer and was also cost effective for this design. The simplicity of this subsystem made for a cleaner and more efficient schematic and layout.

External connectors were divided into input and output groups to clearly define signal direction and improve modularity. UART was selected for external communication because it is simple, reliable, and widely supported for subsystem integration.

To improve testing and usability, a debug button and LED were included. These allow quick verification of system behavior without requiring external debugging tools.

The power system was designed using a 9V input and a LM2575T-3.3G switching regulator to efficiently step down to 3.3V. A switching regulator was selected over a linear regulator to reduce heat dissipation and improve efficiency.

An ICSP interface was also included to allow in-circuit programming and debugging allowing the subsystem to be easily updated during development.

### Requirements Alignment

- Environmental Sensing  
  The TC74 sensor provides digital temperature measurements, fulfilling the subsystem’s sensing requirement.

- Communication  
  I²C (sensor interface) and UART (external interface) enable reliable and efficient data transfer.

- System Integration  
  Clearly defined connector inputs and outputs allow seamless integration with other team subsystems.

- Expandability  
  Available GPIO, PWM, and UART interfaces allow future feature expansion without redesign.

- Debugging Capability  
  The onboard debug LED and button provide immediate feedback during testing.

- Power Reliability  
  The switching regulator ensures stable and efficient 3.3V power delivery.

- Maintainability  
  The ICSP interface supports firmware updates and debugging without removing the board.

# Environmental Sensor Subsystem

## 1. Microcontroller Block

- PIC18F57Q43 — 48-pin SMD

### Sub-blocks / Peripherals

| Peripheral | PIC Pin(s) | Function | Notes |
|-----------|------------|---------|------|
| I²C (MSSP1) | RC3 (SCL), RC4 (SDA) | TC74 Temperature sensor | Bidirectional communication |
| UART | RF0 (TX), RC7 (RX) | External connector communication | Serial communication to external system |
| PWM | RC6 | PWM output signal | Sent to connector |
| GPIO / Digital Input | RC1, RC0 | External digital inputs | From connector |
| GPIO / Digital Output | RB0 | External digital output | To connector |
| GPIO / Digital Input | RB2 | Debug Button | User input |
| GPIO / Digital Output | RF1 | Blue Debug LED | Status indicator |
| ICSP | RB6 (PGEC), RB7 (PGED), MCLR | Microchip SNAP programmer | In-circuit programming |

## 2. Sensor

| Sensor | Measurements | Signal Type | PIC Peripheral | PIC Pin | Notes |
|------|-------------|-------------|---------------|--------|------|
| TC74 | Temperature | Digital – Serial (I²C) | I²C | RC3, RC4 | Single environmental sensor |

## 3. External Connectors

### Connector OUT

| Signal | PIC Pin | Type |
|------|--------|------|
| UART TX | RF0 | Digital – Serial |
| PWM | RC6 | Digital – PWM Output |
| Digital Out | RB0 | Digital Output |

### Connector IN

| Signal | PIC Pin | Type |
|------|--------|------|
| UART RX | RC7 | Digital – Serial |
| Digital Input 1 | RC1 | Digital Input |
| Digital Input 2 | RC0 | Digital Input |

## 4. Debug Interface

| Component | PIC Pin | Type |
|----------|--------|------|
| Debug Button | RB2 | Digital Input |
| Blue Debug LED | RF1 | PWM Digital Output |

## 5. Programming Interface

| Interface | PIC Pins | Notes |
|----------|---------|------|
| ICSP Programmer | RB6 (PGEC), RB7 (PGED), MCLR | Microchip SNAP programming interface |

## 6. Power System

| Component | Description |
|----------|-------------|
| Input Power | 9V unregulated wall adapter |
| Connector | 9V Barrel Jack |
| Voltage Regulator | 3.3 V Switching Power Supply (LM2575T-3.3G) |
| Output | 3.3 V for MCU, sensor, LEDs |

Maximum current: 1.5 A regulator capacity

## 7. Connections / Arrow Labels

- TC74 ↔ PIC  
  `Digital – Serial (I²C, 2 pins)`

- Connector OUT ← PIC  
  `UART TX (1 pin)`  
  `PWM Output (1 pin)`  
  `Digital Output (1 pin)`

- Connector IN → PIC  
  `UART RX (1 pin)`  
  `Digital Input (2 pins)`

- Debug Button → PIC  
  `Digital – Parallel (1 pin)`

- Debug LED ← PIC  
  `Digital – Parallel (1 pin)`

- ICSP Programmer ↔ PIC  
  `Digital – Serial (ICSP, 2 pins)`
