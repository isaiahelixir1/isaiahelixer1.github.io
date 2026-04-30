---
title: Hardware V2.0
---

## Hardware Version 2.0 Overview

The current Version 1.0 Temperature Subsystem PCB successfully integrates a PIC18F57Q43 microcontroller,  
TC74 I2C temperature sensor, LM2575T-3.3G regulator, and a UART daisy-chain communication system.

While the design meets all functional requirements, several improvements could increase reliability,  
debug capability, and long-term scalability.

## Power Supply and Protection Improvements

The current schematic provides a basic 9V to 3.3V conversion stage,  
but Version 2.0 would improve board safety and stability.

Key improvements:
- Add pull-down resistors to prevent floating GPIO and unused MCU pins  
- Improve reverse polarity protection on the input supply  
- Prevent backfeeding voltage from other connected subsystem boards  
- Add better current limiting and voltage protection stages  
- Improve overall power isolation between subsystems  

These changes would increase in multi-board rover building  
and reduce risk of electrical damage during connection errors.

## Microcontroller Optimization and Pin Usage

The current design uses a PIC18F57Q43 in a larger 48-pin package,  
but only a small portion of the available pins are actively used.

In Version 2.0:
- Reduce MCU package size to better match actual pin usage  
- Free unused pins for additional system expansion  
- Improve pin planning for cleaner routing and reduced PCB complexity  

## Communication and Interface Expansion

Although the system currently uses UART and I2C,  
future improvements should increase communication flexibility.

Proposed updates:
- Route additional SPI-capable pins to headers in case I2C fails or expansion is needed  
- Add PWM-capable expansion pins for future actuator or control features  
- Improve routing accessibility for alternate communication protocols  

## Debugging and Testability Improvements

The current board has limited test access points for debugging.

Version 2.0 improvements include:
- Add more test points for power rails, ground, and communication lines  
- Expand access to UART, I2C, and reset signals  
- Add labeled debug pads for faster troubleshooting  
- Improve measurement accessibility for firmware development  

## Sensor and Environmental Expansion

The current design is centered around the TC74 temperature sensor.

In Version 2.0, the system could be expanded to support:
- Wider range environmental sensing capabilities  
- Additional digital or analog sensors  
- More flexible sensor interface options  

This would allow the subsystem to evolve from a single-purpose temperature node  
into a more general environmental monitoring platform.

## Conclusion

Version 2.0 focuses on improving:
- Electrical safety and protection  
- Better use of microcontroller resources  
- Expanded communication flexibility (SPI, PWM, I2C resilience)  
- Increased testability and debugging access  
- Broader environmental sensing capability  
