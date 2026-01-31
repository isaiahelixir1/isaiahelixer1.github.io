---
title: Module's Requirements
---

## Project Requirements Description

This requirements table defines the functional, electrical, and interface constraints for the Environmental Sensor Subsystem. This board is designed as a standalone, modular PCB responsible for collecting environmental data—including CO₂ concentration, temperature, humidity, atmospheric pressure, and ambient light—and transmitting that data to the rover’s daisy-chained UART communication network.

The table captures minimum and target performance levels to ensure the subsystem meets both team-level design goals and course-mandated hardware constraints. These requirements guide component selection, schematic design, firmware development, and system verification prior to integration with other rover subsystems.

## Environmental Sensor Subsystem – Requirements Table

| Requirement Description | Measure of Threshold (Minimum – Not Complete Failure) | Target Measure | Stretch Requirement (Y–N) |
|------------------------|-------------------------------------------------------|----------------|----------------------------|
| Subsystem shall be implemented as a standalone PCB | Single custom PCB with all required components | Modular PCB suitable for daisy-chain integration | No |
| Board shall accept external 9 V power input | Barrel jack adapter accepts 9 V input | Reverse-polarity protected 9 V input | No |
| Board shall include 3.3 V switching regulator | Regulated output ≥3.2 V | Stable 3.3 V output under full load | No |
| Bus power jumper shall be provided | Jumper enables/disables bus power input | Clearly labeled jumper with safe isolation | No |
| Barrel jack to bus power jumper shall be provided | Jumper connects/disconnects barrel jack to bus | Independent power source selection | No |
| Board shall include a surface-mounted microcontroller | One MCU with UART and I2C support | ESP32 running MicroPython | No |
| In-circuit programming support shall be provided | USB or ICSP programming interface present | USB connector for firmware upload and debugging | No |
| Subsystem shall implement UART communication | Can send or receive basic UART messages | Fully compliant daisy-chain UART messaging | No |
| Subsystem shall forward non-addressed UART messages | Messages pass through without corruption | Reliable message forwarding under load | No |
| Subsystem shall measure atmospheric pressure | Pressure sensor provides readable values | Calibrated pressure readings (0–1100 hPa) | No |
| Subsystem shall measure temperature | Temperature readings available | ±1 °C accuracy | No |
| Subsystem shall measure relative humidity | Humidity readings available | ±5% RH accuracy | No |
| Subsystem shall measure CO₂ concentration | CO₂ sensor outputs readable data | Stable CO₂ readings across operating range | No |
| Subsystem shall measure ambient light level | Light sensor provides relative intensity data | Calibrated or normalized light measurements | No |
| At least one sensor shall use serial communication | ≥1 sensor communicates via I2C or SPI | All sensors use digital serial interfaces | No |
| Subsystem shall transmit sensor telemetry | Data available upon request | Periodic telemetry ≥1 Hz | No |
| Subsystem shall not include actuation hardware | No motors or motor drivers present | N/A | No |
| Subsystem current draw shall remain within limits | Total current ≤2 A | ≤1.5 A during normal operation | No |
| Subsystem shall operate in indoor lab conditions | Functional at room temperature | Functional at 0–40 °C, 20–80% RH | No |
| Sensor calibration support | Raw sensor data accessible | Software-based calibration offsets applied | Yes |

## Design Notes and Constraints

- **Power Architecture:** The board supports both bus power and a local 9 V barrel jack input, selectable using onboard jumpers as required by course specifications.
- **Microcontroller:** An ESP32 running MicroPython is used to satisfy sensing, UART communication, and firmware flexibility requirements.
- **Sensors:** The subsystem integrates CO₂, temperature, humidity, pressure, and light sensors using digital serial interfaces (I2C/SPI/UART).
- **Communication:** UART daisy-chain compatibility ensures seamless integration with other team subsystems.
- **Function Scope:** This board performs sensing only and does not replicate actuation, HMI, or wireless communication functionality.
