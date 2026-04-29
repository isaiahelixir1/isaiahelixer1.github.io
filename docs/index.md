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

**Submission: 5/04/2026**
</center>

## Introduction

This datasheet documents the environmental sensor subsystem developed for our interplanetary rover. The purpose of this subsystem is to collect environmental data specifically temperature and transmit it reliably to the rover’s communication network. The document outlines requirements, design decisions, and implementation details that guide the construction and verification of the subsystem as a modular, standalone PCB.

## Project Summary

Our team designed the investigative rover as an open-ended exploration platform to simulate environmental monitoring on a planetary surface. The rover features modular subsystems, each responsible for a different function such as sensing, actuation, human-machine interface (HMI), and communication.

My module, the environmental sensor subsystem, focuses on collecting temperature data using a digital I²C sensor and transmitting it through a UART-based daisy-chain network. This enables reliable communication between subsystem boards and supports real-time telemetry to other modules in the system.

The subsystem is designed to be simple, reliable, and compatible with the team’s shared communication and power architecture.

For full context of the team’s design concept and integrated rover system, see our [team report](https://isaiahelixir1.github.io/EGR314-S-2026-205.github.io/).

## My Contribution

I am responsible for designing, building, and verifying the environmental sensor subsystem. This includes:

- Selecting and integrating the TC74 digital temperature sensor (I²C interface)  
- Implementing UART communication for data transmission within the modular system  
- Designing the power system using a 3.3 V switching regulator (LM2575) with a 9V input  
- Ensuring compatibility with other team modules through standardized connectors  
- Programming and debugging the PIC18F57Q43 microcontroller  

To review detailed information about the components used in the subsystem—including sensor selection, microcontroller configuration, schematic design, PCB design, and power design—see the following links below.  
["Block Diagram"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/02-Block-Diagram/Block-Diagram/)  
["Component Selection"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/03-Component-Selection/Component-Selection/)  
["Microcontroller Selection"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/04-Microcontroller-Selection/Microcontroller/)  
["BOM"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/04-BOM/BOM/)  
["Power Budget"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/06-Power-Budget/Power/)  
["Schematic Design"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/07-Schematic/Schematic/)  
["PCB Design"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/08-PCB-Design/PCB%20Design/)  
