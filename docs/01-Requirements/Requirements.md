---
title: Module's Requirements
---

## Project Requirements Description

The requirements table lists the functional, electrical, and interface constraints for the Environmental Sensor Subsystem of the interplanetary rover. This board is designed as a single, modular PCB responsible for collecting environmental data including gas concentration, temperature, humidity, atmospheric pressure, ambient light, and transmitting that data to the rover’s daisy-chained UART communication network.

The table below shows minimum and target performance levels to ensure the subsystem meets the team level design goals and course hardware constraints. These requirements will help with component selection, schematic design, firmware development, and system verification before integrating this subsystem with other rover boards.

## Environmental Sensor Subsystem Requirements Table

| Requirement Description | Measure of Threshold (Minimum – Not Complete Failure) | Target Measure | Stretch Requirement (Y–N) |
|------------------------|-------------------------------------------------------|----------------|----------------------------|
| Subsystem shall be implemented as a standalone PCB | Single custom PCB with all required components | Modular PCB suitable for daisy-chain integration | No |
| Board shall accept external 9 V power input | Barrel jack adapter accepts 9 V input | Reverse-polarity protected 9 V input | No |
| Board shall include 3.3 V switching regulator | Regulated output ≥3.2 V at ≥300 mA | Stable 3.3 V output at ≥500 mA under full load | No |
| Bus power jumper shall be provided | Jumper enables/disables bus power input | Clearly labeled jumper with safe isolation | No |
| Barrel jack to bus power jumper shall be provided | Jumper connects/disconnects barrel jack to bus | Independent power source selection | No |
| Board shall include a surface-mounted microcontroller | One MCU with UART and I2C support | ESP32 (MicroPython) or SMD PIC with UART and I2C | No |
| In-circuit programming support shall be provided | USB or ICSP programming interface present | USB connector or ICSP header for debugging | No |
| Subsystem shall implement UART communication | Can send or receive UART messages | Fully compliant daisy-chain UART messaging | No |
| Subsystem shall forward non-addressed UART messages | Messages pass through without corruption | Reliable message forwarding under load | No |
| Subsystem shall measure atmospheric pressure | Pressure sensor provides readable values | Calibrated pressure readings (0–1100 hPa) | No |
| Subsystem shall measure temperature | Temperature readings available | ±2 °C accuracy | No |
| Subsystem shall measure relative humidity | Humidity readings available | ±5% RH accuracy | No |
| Subsystem shall measure gas concentration | Gas sensor outputs readable data | Stable gas readings across operating range | No |
| Subsystem shall measure ambient light level | Light sensor provides relative intensity data | Calibrated or normalized light measurements | No |
| At least one sensor shall use serial communication | ≥1 sensor communicates via I2C or SPI | All sensors use digital serial interfaces | No |
| Subsystem shall transmit sensor telemetry | Data available upon request | Periodic telemetry ≥1 Hz | Yes |
| Subsystem current draw shall remain within limits | ≤300 mA during normal operation | ≤500 mA including sensor warm-up and MCU peaks | No |
| Subsystem shall operate in indoor lab conditions | Functional at room temperature | Functional at 0–40 °C, 20–80% RH | No |
| Sensor calibration support | Raw sensor data accessible | Software-based calibration offsets applied | Yes |

## Other Useful Design Notes and Constraints

- Power Build: The board supports both bus power and a local 9 V barrel jack input, selectable using onboard jumpers as required by course specifications.
- Power Budget: The environmental sensor subsystem is low-power by design, with total current draw expected to remain below 500 mA even during sensor warm-up and microcontroller peak usage.
- Microcontroller: The subsystem supports either an ESP32 running MicroPython or a surface-mount PIC microcontroller, provided UART and I2C requirements are met.
- Sensors: The subsystem integrates gas detection, temperature, humidity, pressure, and Light intensity. 
