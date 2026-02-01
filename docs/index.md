---
title: Welcome
tags:
- sensor
- environmental
- rover
---
<center>
<font size="6">Isaiah LaCombe Datasheet</font><br>
as part of<br>
<font size="8">Project X</font><br>
for<br>
<font size="5">Team 305</font><br>

**Submission: 1, 31, 2026**
</center>

## Introduction

This datasheet documents the Environmental Sensor Subsystem developed for our interplanetary rover. The purpose of this subsystem is to collect critical environmental data, including gas concentration, temperature, humidity, atmospheric pressure, and ambient light, and transmit it reliably to the rover’s communication network. The document outlines requirements, design decisions, and implementation details that guide the construction and verification of the subsystem as a modular, standalone PCB.

### Project Summary

Our team designed the rover as an **open-ended exploration platform** to simulate environmental monitoring on a planetary surface. The rover features modular subsystems, each responsible for a different function, such as sensing, actuation, HMI, and wireless communication. My module, the **Environmental Sensor Subsystem**, focuses on collecting and transmitting environmental data through a UART daisy-chain network, enabling real-time telemetry to the base station or local display. This subsystem ensures that the rover can make informed decisions and safely navigate its environment while supporting the team’s mission objectives.

For full context of the team’s design concept and integrated rover system, see our [team report](https://isaiahelixir1.github.io/EGR314-S-2026-205.github.io/).

### My Contribution

I am responsible for designing, building, and verifying the **Environmental Sensor Subsystem**. This includes:

- Selecting and integrating sensors (gas, temperature, humidity, pressure, and light)  
- Implementing UART communication for telemetry in the rover’s modular network  
- Designing the power system with a 3.3 V switching regulator and barrel jack input  
- Ensuring the subsystem is low-power, safe, and compatible with other team modules  
- Programming and debugging the microcontroller (ESP32 or SMD PIC)  

To review detailed information about the components used in the subsystem, including sensor part numbers, microcontroller selection, and power considerations, see the ["BOM"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/04-BOM/BOM/) section of this datasheet. This helps anyone unfamiliar with the system understand exactly what materials and parts were used to implement this module.

