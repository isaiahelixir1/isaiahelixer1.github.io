---
title: Microcontroller Selection
---

The microcontroller is responsible for:

- Reading sensor data via digital communication (I²C)
- Providing UART debugging interface
- Driving LEDs and reading button input
- Communicating with teammate boards via ribbon connector
- Supporting ICSP programming
- Providing general-purpose GPIO control for subsystem integration

## Candidate 1 – PIC18F57Q43 TQFP-48

![download](https://github.com/user-attachments/assets/ff216aa2-51d3-48a9-b728-ff7e14b96ebc)

Manufacturer: Microchip  

Pros | Cons
---|---
Multiple I²C peripherals | 8-bit architecture
Multiple UART modules | No built-in wireless
Sufficient GPIO for subsystem design | Less RAM than 32-bit MCUs
Surface mount TQFP package | —
Supported in MPLAB X / MCC | —

## Candidate 2 – PIC18F47Q10 TQFP-40

![download](https://github.com/user-attachments/assets/eed4b9db-ea31-4c3f-baa7-20b90c00e366)

Manufacturer: Microchip  

Pros | Cons
---|---
Lower cost | Fewer advanced peripherals than Q43
Multiple ADC channels | Lower overall feature set
Familiar architecture | Slightly less flexible for expansion
Surface mount package | —

## Candidate 3 – ESP32 QFN

![20509_2 (1)](https://github.com/user-attachments/assets/923d8fb8-0140-4519-b7af-af5417f9e173)

Manufacturer: Espressif  

Pros | Cons
---|---
32-bit processor | QFN package difficult to solder
Built-in WiFi/BLE | Excessive for subsystem requirements
High processing capability | Higher power consumption
Large ecosystem | More complex firmware stack

## Final Selection: PIC18F57Q43

### Rationale:

The PIC18F57Q43 was selected because it provides the best balance of simplicity, peripheral availability, and system integration capability for the environmental monitoring subsystem.

Key advantages include:
- Multiple I²C modules for reliable digital sensor communication  
- UART support for debugging and external system communication  
- Adequate GPIO for LEDs, buttons, and subsystem control signals  
- ICSP support for in-circuit programming and debugging  
- Suitable surface-mount TQFP package for PCB implementation  

While the 12-bit ADC capability is available, the final system primarily relies on digital sensor communication (I²C), making peripheral availability and integration more important than analog resolution.

The ESP32 was rejected due to unnecessary complexity and power overhead, while the PIC18F47Q10 was less suitable due to reduced peripheral flexibility.

## Summary

The PIC18F57Q43 provides the optimal balance between functionality and design simplicity, meeting all subsystem requirements while maintaining a clean and scalable architecture.  
To see the power budget click here: ["Power Budget"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/06-Power-Budget/Power/) or to see the bill of materials click here: ["BOM"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/04-BOM/BOM/)
