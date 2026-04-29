---
title: Module's Selected Major Components
---

## Overview

The following sections describe the **final selected major components** for the Environmental Monitoring Subsystem. These selections reflect design updates, instructor feedback, and system integration requirements.

This subsystem:

- Operates at 3.3V from a 9V input source  
- Measures environmental temperature  
- Communicates digitally with a microcontroller (I²C)  
- Interfaces with teammate subsystems via external connectors  
- Uses surface-mount components for compact PCB design  

## Power Regulator Selection

### Candidate 1 – MCP1700T-3302E/TT (Linear Regulator)

Pros | Cons
---|---
Low cost | Low efficiency at high input voltage
Low dropout voltage | Heat dissipation concerns
Simple design | Limited current (250 mA)

### Candidate 2 – TLV70033 (Linear Regulator)

Pros | Cons
---|---
Low quiescent current | Limited current (200 mA)
Stable 3.3V output | Slightly higher cost

### Candidate 3 – LM2575T-3.3G (Switching Regulator)

Pros | Cons
---|---
High efficiency | More complex design
Supports high current (up to 1A+) | Requires external components (inductor, diode)
Low heat generation | Larger footprint

### ✅ Final Selection: LM2575T-3.3G

**Rationale:**

The LM2575T-3.3G switching regulator was selected due to its **high efficiency and ability to handle higher current loads** compared to linear regulators.

Although linear regulators offer simplicity, they would dissipate significant power when stepping down from 9V to 3.3V, resulting in heat and reduced efficiency. The switching regulator minimizes these losses, making it more suitable for a stable and scalable system.

This choice ensures:
- Reliable 3.3V regulation  
- Reduced thermal stress  
- Support for future subsystem expansion  

## Environmental Sensor Selection

### Candidate 1 – BME680 (Multi-Sensor)

Pros | Cons
---|---
Multiple measurements (gas, temp, humidity, pressure) | Higher cost
I²C interface | More complex firmware
Compact integration | Overkill for requirements

### Candidate 2 – TC74 (Temperature Sensor)

Pros | Cons
---|---
Simple I²C interface | Only measures temperature
Low pin count | Limited functionality
Reliable digital output | —

### Candidate 3 – HDC1080 (Temp + Humidity)

Pros | Cons
---|---
Low power | More functionality than required
Digital interface | Slightly more complex

### ✅ Final Selection: TC74

**Rationale:**

The TC74 was selected because it **directly meets the subsystem requirement of temperature measurement** without unnecessary complexity.

Compared to multi-sensor options like the BME680, the TC74:
- Reduces firmware complexity  
- Minimizes power consumption  
- Uses fewer system resources  
- Simplifies integration with the microcontroller  

This aligns with the design goal of keeping the subsystem **focused, efficient, and easy to implement**.

## Final Component Summary Table (Required)

| Subsystem | Selected Component | Function |
|----------|------------------|----------|
| Microcontroller | PIC18F57Q43 | System control, communication, and processing |
| Voltage Regulation | LM2575T-3.3G | Steps 9V input down to regulated 3.3V |
| Temperature Sensor | TC74 | Digital temperature measurement (I²C) |

## Design Validation Against Requirements

All selected components meet the subsystem design requirements:

- **3.3V Operation:**  
  All components operate within the regulated 3.3V supply.

- **Efficient Power Conversion:**  
  The switching regulator minimizes energy loss and heat.

- **Functional Accuracy:**  
  The TC74 provides reliable digital temperature readings.

- **System Integration:**  
  I²C communication enables simple and robust interfacing with the microcontroller.

- **Scalability:**  
  The regulator supports higher current, allowing future expansion.

- **Manufacturability:**  
  All components are available in surface-mount packages and have complete documentation.

## MCC Configuration / Pin Usage (PIC18F57Q43)

| Peripheral | Pins Used | Function |
|-----------|----------|---------|
| I²C (MSSP1) | RC3 (SCL), RC4 (SDA) | TC74 communication |
| UART | RF0 (TX), RC7 (RX) | External communication |
| PWM | RC6 | Output signal |
| GPIO Input | RC0, RC1 | External inputs |
| GPIO Output | RB0 | External output |
| Debug Input | RB2 | Button |
| Debug Output | RF1 | LED |
| ICSP | RB6, RB7, MCLR | Programming interface |

## Summary

The final component selections prioritize **efficiency, simplicity, and reliability**. The LM2575T-3.3G ensures stable and efficient power delivery, while the TC74 provides a straightforward and effective temperature sensing solution. Together, these components support a clean, modular, and scalable subsystem design.
