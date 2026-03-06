---
title: Individual Block Diagram
tags:
- tag1
- tag2
---

## Overview

This block diagram documents the layout and connections of my individual environmental monitoring subsystem. Its purpose is to clearly show how power, sensors, actuators, and communication interfaces are organized within the subsystem, and how this subsystem connects to the rest of the team’s boards.  

## Individual Block Diagram — Environmental Sensor Subsystem
![314 Sensor Subsystem (1)](https://github.com/user-attachments/assets/bb1c92bf-5110-4fe3-a8f7-fa1ab782dae9)

## 1. Microcontroller Block

**PIC18F57Q43 — 48-pin SMD**

### Sub-blocks / Peripherals

| Peripheral | PIC Pin(s) | Function | Notes |
|-----------|------------|---------|------|
| I²C (MSSP1) | RC3 (SCL), RC4 (SDA) | BME680 environmental sensor | Bidirectional communication |
| GPIO / Digital Input | RB0 | Debug Button | User input |
| GPIO / Digital Output | RA1 | Blue LED | Status indicator |
| GPIO / Digital Output | RA2 | Red LED | Status indicator |
| UART | RC6 (TX), RC7 (RX) | Debug Serial Header (1×3) | Local debugging |
| ICSP | RB6 (PGC), RB7 (PGD), MCLR | Microchip SNAP Programmer | In-circuit programming |

## 2. Sensor

| Sensor | Measurements | Signal Type | PIC Peripheral | PIC Pin | Notes |
|------|-------------|-------------|---------------|--------|------|
| BME680 | Temperature, Humidity, Pressure, Gas (VOC) | Digital – Serial (I²C) | I²C | RC3, RC4 | Single integrated environmental sensor |

## 3. 2×4 Connector Connections

| Connector Pin | Signal | PIC Pin | Type |
|--------------|--------|--------|------|
| 1 | 3.3 V | VDD | Power |
| 2 | SDA | RC4 | Digital – Serial (I²C) |
| 3 | SCL | RC3 | Digital – Serial (I²C) |
| 4 | UART TX | RC6 | Digital |
| 5 | UART RX | RC7 | Digital |
| 6 | Reserved | — | Optional |
| 7 | Reserved | — | Optional |
| 8 | GND | VSS | Ground |

## 4. Power Supplies

- **3.3 V DC Switching Regulator**
- Supplies power to:
  - PIC18F57Q43
  - BME680 Sensor
  - LEDs
  - External connector

**Maximum current:** 500 mA

## 5. Connections / Arrow Labels (Diagram-Ready)

- BME680 ↔ PIC  
  `Digital – Serial (I²C, 2 pins)`

- PIC ↔ 2×4 Connector  
  `Digital – Serial (I²C pass-through, 2 pins)`

- Debug Button → PIC  
  `Digital – Parallel (1 pin)`

- LEDs → PIC  
  `Digital – Parallel (1 pin each)`

- UART Debug Header ↔ PIC  
  `Digital – Serial (UART, 2 pins)`

- ICSP Programmer ↔ PIC  
  `Digital – Serial (ICSP, 2 pins)`


