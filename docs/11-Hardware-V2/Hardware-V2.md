---
title: Hardware V2.0
---

## Hardware Version 2.0 Overview

The current Version 1.0 Temperature Subsystem PCB successfully integrates a PIC18F57Q43 microcontroller,
TC74 I2C temperature sensor, LM2575T-3.3G regulator and a UART daisy-chain communication system.

While the design meets all functional requirements, several improvements could increase reliability,  
debug capability, and long-term scalability.

## Power Supply Improvements

The current schematic uses an LM2575T-3.3G regulator to convert 9V to 3.3V.

While functional, it is less efficient and produces more ripple than modern buck converters.

In Version 2.0, this could be improved by:
- Replacing with a synchronous buck converter  
- Adding local decoupling capacitors near MCU and TC74  
- Adding test points for 3.3V, GND, and VIN  

These changes would reduce noise and improve I2C stability.

## Debugging and Testability

The current design relies on UART and ICSP for debugging, but has limited hardware visibility.

A Version 2.0 redesign would include:
- UART debug header for live logging  
- Test pads for I2C (SDA/SCL), reset, and UART lines  
- Optional USB-to-UART interface footprint  

This would significantly reduce debugging time during firmware development and integration.

## I2C and Communication Improvements

The TC74 sensor communicates over I2C, but pull-up resistors are not clearly defined  
in the current schematic.

Improvements for Version 2.0:
- Add 4.7 kΩ pull-up resistors on SDA and SCL  
- Add optional series resistors (33–100 Ω)  
- Improve signal integrity for longer traces  

UART daisy-chain communication could also be improved  
with better protection and optional buffering.

## Conclusion

Version 2.0 focuses on improving:
- Power efficiency  
- Debug accessibility  
- Signal integrity  
- Communication robustness  

These changes would make the subsystem more reliable  
and easier to develop while maintaining compatibility  
with the existing TC74 temperature system.
