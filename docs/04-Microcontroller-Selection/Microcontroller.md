---
title: Microcontroller Selection
--- 

The microcontroller is responsible for:

- Reading analog gas sensor data (ADC)
- Communicating with digital sensors via I2C
- Providing UART debugging interface
- Driving LEDs and reading button input
- Communicating with teammate boards via ribbon connector
- Supporting ICSP programming

## Candidate 1 – PIC18F57Q43 (TQFP-40)
![download](https://github.com/user-attachments/assets/ff216aa2-51d3-48a9-b728-ff7e14b96ebc)

Manufacturer: Microchip  

Pros | Cons
---|---
12-bit ADC (ideal for analog gas sensor) | 8-bit architecture
Multiple I2C peripherals | Less RAM than 32-bit MCUs
Multiple UART modules | No built-in wireless
Large pin count (40 pins) | —
Supported in MPLAB X / Melody | —
Surface mount TQFP (easier than QFN) | —

## Candidate 2 – PIC18F47Q10 (TQFP-40)
![download](https://github.com/user-attachments/assets/eed4b9db-ea31-4c3f-baa7-20b90c00e366)

Manufacturer: Microchip  

Pros | Cons
---|---
Lower cost than Q43 series | Slightly fewer advanced peripherals
Multiple ADC channels | Lower ADC resolution (10-bit)
Familiar architecture | —
Surface mount package | —

## Candidate 3 – ESP32 (QFN)
![548508059-04d342bf-6bf9-438c-9624-765d890087fb (1)](https://github.com/user-attachments/assets/e3da2262-6c78-4662-a664-31aafd7af7b5)


Manufacturer: Espressif  

Pros | Cons
---|---
32-bit processor | QFN package difficult to solder
Built-in WiFi/BLE | Overkill for subsystem
High RAM | Higher power consumption
Multiple peripherals | More complex firmware

### Final Selection: PIC18F57Q43

Rationale:

The PIC18F57Q43 provides:

- 12-bit ADC resolution for accurate gas sensing
- Multiple I2C modules for digital sensors
- UART for debugging
- Sufficient GPIO for LEDs and button
- ICSP support for in-circuit programming
- Surface-mount TQFP package that is practical for PCB assembly

It meets all subsystem requirements without adding unnecessary complexity or wireless overhead.
