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
![download (1)](https://github.com/user-attachments/assets/c16e079b-053e-4988-869c-be641a4b35a7)
Pros | Cons
---|---
High efficiency | More complex design
Handles higher current | Requires inductor + more components
Less heat generation | Higher cost

### Final Selection: LM2575T-3.3G
Rationale:  
The system current requirements are within safe limits for a linear regulator. The MCP1700 provides sufficient current at low cost with minimal PCB complexity.
## Gas Sensor Selection
### Candidate 1 – MQ-135 (Analog Output)
<img width="250" height="250" alt="d467641d-68fa-4d95-82e4-fd8358655220" src="https://github.com/user-attachments/assets/c126c38f-8324-4c79-9141-f1d679b21d41" />
Pros | Cons
---|---
Simple analog interface | Requires calibration
Widely documented | Heater consumes significant current
Low cost | Lower precision

### Candidate 2 – CCS811 (Digital I2C Gas Sensor)
<img width="400" height="400" alt="F5XGEDTKZMPKIDT" src="https://github.com/user-attachments/assets/4c444bfd-9500-4c48-8888-8d98443d08bd" />
Pros | Cons
---|---
Digital I2C interface | More expensive
Lower power consumption | Requires initialization
Integrated air quality algorithm | —

### Candidate 3 – BME680 (Gas + Temp + Humidity + Pressure)
![download](https://github.com/user-attachments/assets/e714dd1e-4268-4e03-9f55-03df74a9a4b1)
Pros | Cons
---|---
Multi-sensor in one chip | More complex firmware
Digital I2C | Higher cost
Small footprint | —

### Final Selection: CCS811
Rationale:  
The CCS811 provides digital air quality readings over I2C, reducing analog noise concerns and simplifying calibration compared to MQ-135.
## Temperature & Humidity Sensor Selection
### Candidate 1 – DHT22
<img width="250" height="250" alt="DHT22-Humidity-sensor" src="https://github.com/user-attachments/assets/3736539a-ea7d-4cfb-8c1e-68e2cfa9d0cf" />
Pros | Cons
---|---
Low cost | Slower response
Simple protocol | Not fully surface mount friendly
Moderate accuracy | —

### Candidate 2 – SHT31 (I2C)
![548490836-bf8cc1a2-3039-4998-a6d6-2fb6b2eacad9 (1)](https://github.com/user-attachments/assets/2a309ab0-5fa2-4ea6-a07a-f533d4417d33)

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
## Light Sensor Selection
### Candidate 1 – Photoresistor (Analog LDR)
<img width="200" height="200" alt="photoresistor_LDR" src="https://github.com/user-attachments/assets/2fb888eb-ecda-42c4-ab6e-c73884d67cb5" />
Pros | Cons
---|---
Very inexpensive | Requires ADC channel
Simple design | Lower accuracy
Easy to source | Affected by temperature

### Candidate 2 – BH1750 (I2C)
![548487120-94a84b46-b59e-4205-80e0-a3e862f127a6 (1)](https://github.com/user-attachments/assets/e6664568-a1e6-422a-99b8-1f41851bd723)
Pros | Cons
---|---
Digital output | Requires I2C bus
High resolution | —
Low power | —

### Candidate 3 – TSL2561 (I2C)
![548487237-e4fae6bd-95fb-4df2-b154-a9a4543407df (1)](https://github.com/user-attachments/assets/1077b8dc-d33e-4810-ab2f-31f5ffec677b)
Pros | Cons
---|---
Wide dynamic range | Slightly higher cost
Digital I2C | More configuration needed

### Final Selection: BH1750
Rationale:  
The BH1750 provides high-resolution digital light measurement with minimal configuration and simple I2C integration.
## Barometric Pressure Sensor Selection
### Candidate 1 – BMP280
![548496325-29767e5d-8bb1-4197-b3e1-86f202d03071 (1)](https://github.com/user-attachments/assets/2042f878-0a57-459f-8089-7edd9c336db7)
Pros | Cons
---|---
Accurate readings | Requires calibration constants
Digital I2C | —
Low power | —

### Candidate 2 – BME280 (Pressure + Temp + Humidity)
![548496521-0c4cdcb8-7e6e-4400-a57e-ff8b6ffa987b (1)](https://github.com/user-attachments/assets/e9ed2578-b234-4399-85b2-7d49d1ad8315)
Pros | Cons
---|---
Multi-function sensor | Redundant if separate sensors used
Compact | Higher cost

### Candidate 3 – MPL3115A2
![548496680-9d8ed861-8663-4720-923e-d3a464860362 (1)](https://github.com/user-attachments/assets/dfaf4959-44bd-422b-b299-6751e32c1af4)
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
