---
title: Module's Selected Major Components
---

The following sections describe the selected major components needed for the Environmental Monitoring Subsystem.

This subsystem:

- Operates entirely at 3.3V with a 9V source
- Measures temperature
- Communicates sensor data digitally and analog to a microcontroller
- Sends processed data to teammate boards via a 2×4 ribbon connector
- all sensors are surface mount components
## Power Regualtor Selection
### Candidate 1 – MCP1700T-3302E/TT (Linear Regulator)
![download](https://github.com/user-attachments/assets/e701130d-f395-4e78-90e9-a419f28889c7)

Pros | Cons
---|---
Low cost | Less efficient than switching
Low dropout voltage | Heat dissipation at higher input voltages
Simple design | Limited current output (250 mA)

### Candidate 2 – TLV70033 (Linear Regulator)
![download](https://github.com/user-attachments/assets/c54c6d3c-61f7-441e-80d4-5d749c8edf84)

Pros | Cons
---|---
Ultra-low quiescent current | Slightly higher cost
Stable 3.3V output | Limited current (200 mA)

### Candidate 3 – LM2575T-3.3G (Switching Regulator)
<img width="605" height="430" alt="Screenshot 2026-04-29 091820" src="https://github.com/user-attachments/assets/677d67d7-06b8-4962-b203-0ed13a477735" />

Pros | Cons
---|---
High efficiency | More complex design
Handles higher current | Requires inductor + more components
Less heat generation | Higher cost

### Final Selection: LM2575T-3.3G
Rationale:  
The system current requirements are within safe limits for a linear regulator. The MCP1700 provides sufficient current at low cost with minimal PCB complexity.
## Gas Sensor Selection

## Environmental Sensor Selection

### Candidate 3 – BME680 (Gas + Temperature + Humidity + Pressure)

<img width="250" height="250" alt="BME680 Sensor" src="https://github.com/user-attachments/assets/e714dd1e-4268-4e03-9f55-03df74a9a4b1" />

Pros | Cons
---|---
Measures gas, temperature, humidity, and pressure | More complex firmware
Digital I2C interface | Slightly higher cost
Single integrated environmental sensor | Requires sensor library
Small footprint | Gas output is resistance-based (requires interpretation)

## Final Selection – BME680

**Rationale:**

The BME680 was selected because it integrates four environmental sensors into a single device, allowing the system to measure gas resistance (air quality indicator), temperature, humidity, and barometric pressure using a single I²C interface.
Using the BME680 significantly simplifies hardware design by eliminating the need for multiple discrete sensors and reducing the number of required microcontroller pins. Although the firmware implementation is slightly more complex, the integrated design results in a smaller PCB footprint, lower overall component count, and easier system integration.

Pros | Cons
---|---
Low cost | Slower response
Simple protocol | Not fully surface mount friendly
Moderate accuracy | —

### Candidate 2 – SHT31 (I2C)
![548495597-2a309ab0-5fa2-4ea6-a07a-f533d4417d33 (1)](https://github.com/user-attachments/assets/78523e99-4278-4789-a006-f663a4c49809)

Pros | Cons
---|---
High accuracy | Higher cost
Fully digital I2C | —
Surface mount package | —

### Candidate 3 – HDC1080 (I2C)
![548487120-94a84b46-b59e-4205-80e0-a3e862f127a6 (1)](https://github.com/user-attachments/assets/9a7fe5a5-11b9-47f1-ad20-83e4287ed701)

Pros | Cons
---|---
Low power | Slightly lower accuracy than SHT31
Small footprint | —
Digital interface | —

### Final Selection: SHT31
Rationale:  
The SHT31 offers high accuracy, reliable I2C communication, and strong industry support while meeting surface mount requirements.

Subsystem | Selected Component
---|---
Voltage Regulation | MCP1700T-3302E/TT
Environmental Sensor (Gas, Temperature, Humidity, Pressure) | BME680

All selected components:
- Operate at 3.3V  
- Support digital I2C or analog interfacing  
- Are surface mount compatible  
- Have complete datasheets  
- Meets subsystem design constraints  
