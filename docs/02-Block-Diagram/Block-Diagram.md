---
title: Individual Block Diagram
tags:
- tag1
- tag2
---
## Individual Block Diagram — Environmental Sensor Subsystem

## Overview

This block diagram documents the layout and connections of my individual environmental monitoring subsystem.  
Its purpose is to clearly show how power, sensors, actuators, and communication interfaces are organized within the subsystem, and how this subsystem connects to the rest of the team’s boards.  

## 1. Microcontroller Block

**PIC18F57Q43 — 40-pin Nano**

### Sub-blocks / Peripherals

| Peripheral | PIC Pin(s) | Function | Notes |
|-----------|------------|---------|------|
| ADC | RA0 (AN0) | Gas sensor analog input | Converts gas sensor voltage to digital internally |
| GPIO / Digital Output | RD1 | Gas sensor digital output to connector | Sends processed gas value to 2×4 connector |
| I²C (MSSP1) | RC3 (SCL), RC4 (SDA) | Temp/Humidity, Light, Pressure sensors | Shared bidirectional bus; also connects to 2×4 connector |
| GPIO / Digital Input | RB0 | Debug Button | User input |
| GPIO / Digital Output | RA1 | Blue LED | Status indicator |
| GPIO / Digital Output | RA2 | Red LED | Status indicator |
| UART | RC6 (TX), RC7 (RX) | Debug Serial Header (1×3) | Local debug only |
| ICSP | RB6 (PGC), RB7 (PGD), MCLR | Microchip SNAP Programmer | In-circuit programming |

## 2. Sensors

| Sensor | Signal Type | PIC Peripheral | PIC Pin | Notes |
|--------|------------|----------------|--------|------|
| Gas Sensor | Analog | ADC | RA0 | Converted digitally inside PIC; digital output sent via RD1 |
| Temperature & Humidity | Digital – Serial (I²C) | I²C | RC3, RC4 | Shared bus |
| Light Intensity | Digital – Serial (I²C) | I²C | RC3, RC4 | Shared bus |
| Barometric Pressure | Digital – Serial (I²C) | I²C | RC3, RC4 | Shared bus |

## 3. 2×4 Connector (To Other Boards)

| Connector Pin | Signal | PIC Pin | Type |
|--------------|--------|--------|------|
| 1 | 3.3 V | VDD | Power |
| 2 | GND | VSS | Ground |
| 3 | SDA | RC4 | Digital – Serial (I²C, 2 pins) |
| 4 | SCL | RC3 | Digital – Serial (I²C, 2 pins) |
| 5 | Gas Digital Out | RD1 | Digital – Parallel (1 pin) |
| 6 | Reserved / Optional | — | — |
| 7 | Reserved / Optional | — | — |
| 8 | GND | VSS | Ground |

## 4. Power Supplies

- **3.3 V DC (regulated)** → All sensors, PIC, LEDs, buttons  
- **Max current**: [Insert your supply rating]  

**Dashed-line box in diagram:** Place around all 3.3 V devices.

## 5. Connections / Arrow Labels (Diagram-Ready)

- **Gas Sensor → RA0:** `Analog (0–3.3 V, 1 pin)`  
- **RD1 → 2×4 Connector:** `Digital – Parallel (1 pin)`  
- **Temp/Humidity → PIC ↔ 2×4 Connector:** `Digital – Serial (I²C, 2 pins)`  
- **Light → PIC ↔ 2×4 Connector:** `Digital – Serial (I²C, 2 pins)`  
- **Pressure → PIC ↔ 2×4 Connector:** `Digital – Serial (I²C, 2 pins)`  
- **Debug Button → PIC:** `Digital – Parallel (1 pin)`  
- **LEDs → PIC:** `Digital – Parallel (1 pin each)`  
- **UART Debug Header → PIC:** `Digital – Serial (UART, 2 pins)`  
- **ICSP → PIC:** `Digital – Serial (ICSP, 2 pins)`  

## 6. Notes / Observations

- All sensors are in separate blocks.  
- Gas sensor uses analog input; digital value sent to connector.  
- I²C bus is bidirectional; multiple sensors share RC3/RC4.  
- Power supply box includes all components at 3.3 V regulated.  
- Connector carries only **digital sensor data and power**, not analog lines.  
- All pins on PIC are accounted for and labeled in the diagram.  
- System ready for ICSP programming, UART debug, and future expansion.  

**Design Summary Sentence:**  
> *The gas sensor provides an analog voltage to RA0 on the PIC18F57Q43, which is converted to a digital value and transmitted via RD1 to the 2×4 connector, while other environmental sensors communicate digitally over I²C to the connector.*

