---
title: Module's Selected Major Components
---

The following sections describe the selected major components needed for the Environmental Monitoring Subsystem.

This subsystem:

- Operates entirely at 3.3V with a 9V source
- Measures gas concentration, temperature, humidity, light intensity, and barometric pressure
- Communicates sensor data digitally and analog to a microcontroller
- Sends processed data to teammate boards via a 2×4 ribbon connector
- all sensors are surface mount components

## Power Regualtor Selection

## Candidate 1 – MCP1700T-3302E/TT (Linear Regulator)

Pros | Cons
---|---
Low cost | Less efficient than switching
Low dropout voltage | Heat dissipation at higher input voltages
Simple design | Limited current output (250 mA)

## Candidate 2 – TLV70033 (Linear Regulator)

Pros | Cons
---|---
Ultra-low quiescent current | Slightly higher cost
Stable 3.3V output | Limited current (200 mA)

## Candidate 3 – LM2575T-3.3G (Switching Regulator)

Pros | Cons
---|---
High efficiency | More complex design
Handles higher current | Requires inductor + more components
Less heat generation | Higher cost

### Final Selection: LM2575T-3.3G

Rationale:  
The system current requirements are within safe limits for a linear regulator. The MCP1700 provides sufficient current at low cost with minimal PCB complexity.

## Gas Sensor Selection

## Candidate 1 – MQ-135 (Analog Output)

Pros | Cons
---|---
Simple analog interface | Requires calibration
Widely documented | Heater consumes significant current
Low cost | Lower precision

## Candidate 2 – CCS811 (Digital I2C Gas Sensor)

Pros | Cons
---|---
Digital I2C interface | More expensive
Lower power consumption | Requires initialization
Integrated air quality algorithm | —

## Candidate 3 – BME680 (Gas + Temp + Humidity + Pressure)

Pros | Cons
---|---
Multi-sensor in one chip | More complex firmware
Digital I2C | Higher cost
Small footprint | —

### Final Selection: CCS811

Rationale:  
The CCS811 provides digital air quality readings over I2C, reducing analog noise concerns and simplifying calibration compared to MQ-135.

## Temperature & Humidity Sensor Selection

## Candidate 1 – DHT22

Pros | Cons
---|---
Low cost | Slower response
Simple protocol | Not fully surface mount friendly
Moderate accuracy | —

## Candidate 2 – SHT31 (I2C)

Pros | Cons
---|---
High accuracy | Higher cost
Fully digital I2C | —
Surface mount package | —

## Candidate 3 – HDC1080 (I2C)

Pros | Cons
---|---
Low power | Slightly lower accuracy than SHT31
Small footprint | —
Digital interface | —

### Final Selection: SHT31

Rationale:  
The SHT31 offers high accuracy, reliable I2C communication, and strong industry support while meeting surface mount requirements.

# Light Sensor Selection

## Candidate 1 – Photoresistor (Analog LDR)

Pros | Cons
---|---
Very inexpensive | Requires ADC channel
Simple design | Lower accuracy
Easy to source | Affected by temperature

### Candidate 2 – BH1750 (I2C)

Pros | Cons
---|---
Digital output | Requires I2C bus
High resolution | —
Low power | —

## Candidate 3 – TSL2561 (I2C)

Pros | Cons
---|---
Wide dynamic range | Slightly higher cost
Digital I2C | More configuration needed

### Final Selection: BH1750

Rationale:  
The BH1750 provides high-resolution digital light measurement with minimal configuration and simple I2C integration.

## Barometric Pressure Sensor Selection

## Candidate 1 – BMP280

Pros | Cons
---|---
Accurate readings | Requires calibration constants
Digital I2C | —
Low power | —

## Candidate 2 – BME280 (Pressure + Temp + Humidity)

Pros | Cons
---|---
Multi-function sensor | Redundant if separate sensors used
Compact | Higher cost

## Candidate 3 – MPL3115A2

Pros | Cons
---|---
Integrated altitude calculation | Slightly higher cost
Digital I2C | —

### Final Selection: BMP280

Rationale:  
The BMP280 provides accurate pressure readings with low power consumption and simple I2C integration while avoiding redundancy with separate temperature and humidity sensors.

## Final Component Summary

Subsystem | Selected Component
---|---
Voltage Regulation | MCP1700T-3302E/TT
Gas Sensor | CCS811
Temperature & Humidity | SHT31
Light Sensor | BH1750
Pressure Sensor | BMP280

All selected components:

- Operate at 3.3V  
- Support digital I2C or analog interfacing  
- Are surface mount compatible  
- Have complete datasheets  
- Meets subsystem design constraints  
