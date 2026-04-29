---
title: Module's Requirements
---

## Project Requirements Description

The requirements table defines the functional, electrical, and interface constraints for the Temperature Subsystem of the interplanetary rover. This board is implemented as a single modular PCB responsible for measuring temperature using a TC74 digital temperature sensor and transmitting that data to the rover’s system communication network.

The subsystem is designed around a regulated 3.3V power rail, a microcontroller PIC18F57Q43, and I2C-based temperature sensing. The requirements below guide component selection, schematic design, firmware development, and system validation prior to integration with other rover subsystems.


## Temperature Subsystem Requirements Table

| Requirement Description | Measure of Threshold (Minimum) | Target Measure | Stretch Requirement (Y or N) |
|------------------------|---------------------------------|----------------|-----------------------------|
| Subsystem shall be implemented as a standalone PCB | Single custom PCB with all required components | Modular PCB suitable for system integration | No |
| Board shall accept external 9 V power input | Barrel jack accepts 9 V DC input | Reverse-polarity protected input stage | No |
| Board shall include 3.3 V voltage regulation | Regulated output ≥3.2 V at ≥300 mA | Stable 3.3 V output at ≥500 mA under load | No |
| Power input protection shall be included | Fuse and protection diode present | Fully protected against reverse polarity and overcurrent | No |
| Board shall include a surface-mounted microcontroller | One MCU with I2C support | PIC18F57Q43 or equivalent MCU | No |
| In-circuit programming support shall be provided | ICSP header present | Dedicated programming/debug interface | No |
| Subsystem shall measure temperature | TC74 temperature sensor provides readable output | ±2 °C accuracy over operating range | No |
| Temperature sensor shall use serial communication | I2C communication required | Stable and noise-resistant I2C communication | No |
| Subsystem shall transmit temperature data | Data accessible via communication interface | Periodic updates ≥1 Hz | Yes |
| Subsystem shall operate in lab environment | Functional at room temperature | Operates from 0–40 °C | No |
| Subsystem current draw shall remain within limits | ≤300 mA normal operation | ≤500 mA peak conditions | No |
| Subsystem shall support modular integration | Standard connector interface provided | Robust ribbon cable communication support | No |

To see the Block Diagram page click here: ["Block Diagram"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/02-Block-Diagram/Block-Diagram/).
