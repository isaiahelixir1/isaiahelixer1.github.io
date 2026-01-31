---
title: Module's Requirements
---

## Project Requirements Description

This table defines the functional and design requirements for the Environmental Sensor Board. The purpose of this module is to collect environmental and atmospheric data, including CO₂ concentration, temperature, humidity, pressure, and ambient light levels, to support the rover’s scientific exploration objective. These requirements ensure the module can reliably gather sensor data, communicate with other rover subsystems over the UART network, and operate within the rover’s power and environmental constraints. Defining these requirements early helps guide component selection, schematic design, and firmware development before system integration.

## Environmental Sensor Board – Requirements Table

| Requirement Description | Measure of Threshold (Minimum – Not Complete Failure) | Target Measure | Stretch Requirement (Y–N) |
|------------------------|-------------------------------------------------------|----------------|----------------------------|
| Module shall operate from rover power system | Accepts regulated input voltage (3.0–3.6 V) | Stable 3.3 V operation with onboard regulation | No |
| Surface-mounted 3.3 V voltage regulator | Output ≥3.2 V under load | Regulated 3.3 V output | No |
| Surface-mounted microcontroller | One MCU capable of UART and I2C | ESP32 or similar MCU with multiple serial interfaces | No |
| Microcontroller power consumption | Operates within system limits | Optimized low-power operation (<150 mA typical) | No |
| Module shall measure atmospheric pressure | Functional pressure readings available | Calibrated pressure readings (0–1100 hPa) | No |
| Module shall measure ambient temperature | Temperature readings available | ±1 °C accuracy | No |
| Module shall measure relative humidity | Humidity readings available | ±5% RH accuracy | No |
| Module shall measure CO₂ concentration | CO₂ sensor provides readable values | Stable CO₂ readings within sensor operating range | No |
| Module shall measure ambient light level | Light sensor provides relative brightness data | Calibrated lux or relative light measurements | No |
| Sensors shall communicate digitally | At least one serial sensor (I2C, SPI, or UART) | All sensors on shared serial buses where possible | No |
| Module shall transmit sensor data over UART | Responds to data request commands | Periodic telemetry at ≥1 Hz | No |
| Module shall support UART daisy-chain networking | Passes messages not addressed to it | Fully compliant daisy-chain behavior | No |
| Module shall operate without motors | No motor control required | N/A | No |
| Module shall remain within current limits | Total draw ≤2 A | ≤1.5 A during normal operation | No |
| Module shall function in lab conditions | Operates indoors | Operates at 0–40 °C, 20–80% RH | No |
| Sensor calibration support | Raw sensor data available | Software calibration offsets applied | Yes |

## Notes on Design Constraints

- **Power:** The module is powered from the rover’s regulated supply and includes an onboard 3.3 V regulator to support the microcontroller and sensors.  
- **Microcontroller:** A microcontroller with UART and I2C support is required to communicate with the rover network and onboard sensors.  
- **Sensors:** The board integrates CO₂, temperature, humidity, pressure, and light sensors using digital interfaces (I2C, SPI, or UART).  
- **Motors:** This module does not control motors and does not require motor drivers or high-current power stages.  
- **Networking:** The module must safely forward UART messages while responding only to messages addressed to it.
