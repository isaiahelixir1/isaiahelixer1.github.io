---
title: Temperature Subsystem PCB Design
---

## Overview

This PCB Design represents the final product of the Environmental Sensor Subsystem Board. The system converts a 9V DC wall adapter input into a regulated 3.3V supply using an LM2575T-3.3G switching regulator with input protection fuse and diode. The regulated rail powers a PIC18F57Q43 microcontroller and a TC74 digital temperature sensor.

The microcontroller processes temperature data from the sensor and communicates with other subsystem boards through a ribbon connector interface. The design includes programming ICSP, debug interfaces, and expansion I/O to support system integration and testing.

Key design priorities include:
- Stable and efficient 3.3V power regulation  
- Reliable I2C temperature sensing TC74 sensor  
- Robust inter-board communication via ribbon connectors  
- Input protection fuse and reverse polarity protection  
- Ease of programming and debugging  

## Updated PCB Design

### Raw PCB (Before Assembly)


<img width="700" height="700" alt="Raw PCB Front" src="https://github.com/user-attachments/assets/adb2db44-f593-480b-bc92-814822522562" />
*Figure 1: Showing the raw PCb front of the board.*
 
<img width="700" height="700" alt="Raw PCB Back" src="https://github.com/user-attachments/assets/26821738-0b3f-4a78-aabf-ed0341fb1169" />
*Figure 2: Showing the raw PCb back of the board.*

### Final PCB (After Soldering & Testing)

<img width="700" height="700" alt="20260429_172211" src="https://github.com/user-attachments/assets/8304a5b6-437e-4906-b472-9818d78ce78f" />
*Figure 3: Showing the soldered front of the board.*
  
<img width="700" height="700" alt="20260429_172218" src="https://github.com/user-attachments/assets/11b84701-5ce7-49d4-9908-537bc7bba867" />
*Figure 4: Showing the soldered back of the board.*

### ECAD Design Views

<img width="700" height="700" alt="CAD PCB Front" src="https://github.com/user-attachments/assets/fdce6b7a-13f8-4c00-9d13-cd1764e72bd4" />
*Figure 5: Showing the CAD model front of the board.*

<img width="700" height="700" alt="CAD PCB Back" src="https://github.com/user-attachments/assets/73387086-86c3-41bd-a2ac-dd38ce98db2c" />
*Figure 6: Showing the CAD model back of the board.*

<img width="700" height="700" alt="CAD PCB 3D" src="https://github.com/user-attachments/assets/bd99cdee-a543-41fa-8610-7ab3ebfdda65" />
*Figure 7: Showing a 3D CAD model of the board.*

## Resources

- [ECAD Project Files (KiCad)](https://github.com/user-attachments/files/27212208/314.Temperature.Sensor.Subsystem.zip)
- [Manufacturing Files (Gerbers and Drill)](https://github.com/user-attachments/files/27213939/314.Temperature.Sensor.Subsystem.Gerber.and.Drill.zip)
- Too see the Microcontroller Code page click here: [Microcontroller Code](https://isaiahelixir1.github.io/isaiahelixer1.github.io/09-Microcontroller-Code/Microcontroller%20Code/)

## Notes

This board was fabricated, assembled, and tested to confirm correct voltage regulation, sensor communication over I2C, and stable system integration with the microcontroller subsystem.
