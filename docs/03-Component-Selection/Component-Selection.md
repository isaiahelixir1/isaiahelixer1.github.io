---
title: Module's Selected Major Components
---

## Overview

The following sections describe the final selected major components not including microcontroller selection for the Temperature Monitoring Subsystem. These selections show updated design decisions, instructor feedback, and system requirements.

This subsystem:

- Operates at 3.3V from a 9V source  
- Measures temperature  
- Communicates digitally with a microcontroller (I²C)  
- Interfaces with teammate boards via connectors  
- Uses surface-mount components  

## Power Regulator Selection

### Candidate 1 – MCP1700T-3302E/TT Linear Regulator

![download](https://github.com/user-attachments/assets/e701130d-f395-4e78-90e9-a419f28889c7)

Pros | Cons
---|---
Low cost | Low efficiency at high input voltage
Low dropout voltage | Heat dissipation at 9V input
Simple design | Limited current (250 mA)

### Candidate 2 – TLV70033 Linear Regulator

![download](https://github.com/user-attachments/assets/c54c6d3c-61f7-441e-80d4-5d749c8edf84)

Pros | Cons
---|---
Low quiescent current | Limited current (200 mA)
Stable 3.3V output | Slightly higher cost

### Candidate 3 – LM2575T-3.3G Switching Regulator

<img width="305" height="230" alt="LM2575" src="https://github.com/user-attachments/assets/677d67d7-06b8-4962-b203-0ed13a477735" />

Pros | Cons
---|---
High efficiency | More complex design
Handles higher current | Requires inductor + diode
Low heat generation | Larger footprint

### Final Selection: LM2575T-3.3G

Rationale:

The LM2575T-3.3G was selected due to its high efficiency and ability to handle higher current loads when stepping down from 9V to 3.3V.

Linear regulators were considered although they would dissipate significant power as heat due to the large voltage drop. The switching regulator minimizes these losses and improves system efficiency and reliability.

This selection ensures:
- Stable 3.3V output  
- Reduced thermal stress  
- Support for future expansion  

## Environmental Sensor Selection

### Candidate 1 – BME680 Gas + Temperature + Humidity + Pressure

<img width="250" height="250" alt="BME680 Sensor" src="https://github.com/user-attachments/assets/e714dd1e-4268-4e03-9f55-03df74a9a4b1" />

Pros | Cons
---|---
Multiple sensing capabilities | Higher cost
I²C interface | More complex firmware
Compact integration | Overkill for requirements

### Candidate 2 – TC74 (Temperature Sensor)

<img width="197" height="197" alt="TC74 Sensor" src="https://github.com/user-attachments/assets/303597e4-1458-4c94-834d-cd8380258836" />

Pros | Cons
---|---
Simple I²C interface | Only measures temperature
Low pin count | Limited functionality
Reliable digital output | —

### Candidate 3 – HDC1080 Temperature + Humidity

![HDC1080](https://github.com/user-attachments/assets/9a7fe5a5-11b9-47f1-ad20-83e4287ed701)

Pros | Cons
---|---
Low power | More features than required
Digital interface | Slightly more complex

### Final Selection – TC74

Rationale:

The TC74 was selected because it directly satisfies the subsystem requirement of temperature measurement without unnecessary complexity.

Compared to multi-sensor devices like the BME680, the TC74:
- Simplifies firmware development  
- Reduces power consumption  
- Minimizes pin usage  
- Keeps the subsystem focused and efficient  

## Final Component Summary Table (Required)

| Subsystem | Selected Component | Function |
|----------|------------------|----------|
| Voltage Regulation | LM2575T-3.3G | Converts 9V input to regulated 3.3V |
| Temperature Sensor | TC74 | Digital temperature measurement (I²C) |

## Design Validation

All selected components meet system requirements:

- 3.3V Compatibility: All components operate at the regulated voltage  
- Efficient Power Conversion: Switching regulator reduces losses  
- Functional Accuracy: TC74 provides reliable temperature data  
- System Integration: I²C simplifies communication  
- Scalability: Regulator supports higher current if needed  
- Manufacturability: All components are surface-mount and well-documented  

## Summary

The final design prioritizes efficiency, simplicity, and reliability. The LM2575T-3.3G provides stable and efficient power conversion while the TC74 offers a straightforward and effective sensing solution. Together these main comonents support a clean and modular subsystem design.  
To see the microcontroller selection page, click here ["Micocontroller Selection"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/04-Microcontroller-Selection/Microcontroller/).
