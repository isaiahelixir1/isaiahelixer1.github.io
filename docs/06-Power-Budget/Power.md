---
title: Power Budget
---

The Environmental Monitoring Subsystem operates from a regulated 3.3V rail derived from a shared 9V system input.

Unlike a standalone design, this subsystem is part of a multi-board system where one board supplies the 9V input rail to upstream connectors. Therefore, the power budget must account for both:

- Local subsystem consumption  
- External load draw through shared power/communication interfaces  

# Section A – Major Component Power Consumption

The following components represent the final design load only.

| Component | Voltage | Max Current (mA) | Notes |
|------------|--------|------------------|------|
| PIC18F57Q43 | 3.3V | 50 mA | Worst-case active operation |
| TC74 Temperature Sensor | 3.3V | 2 mA | I²C digital sensor |
| UART Interface (internal + driver load) | 3.3V | 10 mA | Communication overhead |
| External Connector Logic Load | 3.3V | 10 mA | Pull-downs, receiver input, interface circuitry |

### Local Subsystem Total (No Margin)

50 + 2 + 10 + 10 = 72 mA

# Section B – External System Load Contribution

Because this board supplies and shares a 9V rail through external connectors, additional load must be considered from connected subsystems.

### Estimated External Board Load (Worst Case)

| Source | Estimated Current | Notes |
|--------|------------------|------|
| Downstream teammate board logic + sensors | 150 mA | Estimated shared subsystem load |
| Communication interface overhead | 20 mA | UART line drivers + buffering |

### External Load Total

170 mA

# Section C – Total System Load (Combined)

### Total Current Demand:

- Local subsystem: 72 mA  
- External subsystem load: 170 mA  

### Total System Load:

242 mA

### Safety Margin (25%)

242 mA × 1.25 = 303 mA

### Final Required 3.3V Rail Capacity:

≥ 350 mA design target

# Section D – Voltage Regulator Selection

## Candidate 1 – MCP1700 (Linear Regulator)

| Pros | Cons |
|------|------|
| Simple design | Insufficient headroom for system-wide load |
| Low cost | Poor efficiency from 9V input |
| Small footprint | No scalability for shared system load |

## Candidate 2 – LM2575-3.3G (Switching Regulator)

| Pros | Cons |
|------|------|
| High efficiency | Requires external components |
| Supports up to 1A output | Larger PCB footprint |
| Low thermal dissipation | Slightly more complex design |
| Strong system-level margin | — |

### Final Selection: LM2575-3.3G

### Rationale:

Although the local subsystem alone only requires ~72 mA, the system-level architecture includes shared power distribution to other boards via external connectors.

When including estimated external subsystem loads, total demand increases to approximately 242 mA, and 303 mA with safety margin.

The LM2575-3.3G was selected because it:
- Supports system-level current demand with large headroom  
- Maintains efficiency when stepping down from 9V  
- Reduces heat compared to linear regulators  
- Provides scalability for additional connected modules  

# Section E – 9V Power Source Analysis

## Selected Source:
9V DC Wall Adapter (System Shared Rail)

Rated Current: 3A

### Input Power Estimation

P_out = 3.3V × 0.303A  
P_out ≈ 1.00 W  

Assuming 80% efficiency:

P_in = 1.00W / 0.80  
P_in ≈ 1.25 W  

### Input Current from 9V Rail

I_in = 1.25W / 9V  
I_in ≈ 0.14 A  

### Supply Margin Check

Adapter Rating: 3.0 A  
Required Current: 0.14 A  

Available Headroom: ~2.86 A  

The system power supply is therefore significantly oversized, ensuring stable operation even with multiple connected subsystems.

# Section F – Power Budget Methodology (Required Explanation)

The power budget was constructed using a system-level worst-case analysis approach, rather than only considering the local subsystem.

### Method Used:

1. Identify all local microcontroller and sensor loads  
2. Add estimated interface and communication overhead  
3. Include external subsystem loads connected via shared 9V rail  
4. Apply a 25% safety margin for:
   - simultaneous operation of subsystems  
   - startup current spikes  
   - future expansion  
5. Validate against regulator and supply limits  

### Key Conclusions:

- The subsystem alone is low power (~72 mA), but system integration significantly increases total load.
- External board interaction is the dominant contributor to total current consumption.
- A linear regulator is not suitable due to:
  - system-level current demand  
  - inefficiency from 9V input  
- The LM2575 switching regulator provides necessary **system-wide scalability and thermal stability**
- The design is safely within supply limits even when multiple subsystems operate simultaneously

# Final Power System Summary

| Rail | Voltage | Total Required Current | Regulator |
|------|--------|------------------------|------------|
| System 3.3V Rail | 3.3V | 350 mA (design target) | LM2575-3.3G |

The final power architecture ensures that both local and system-wide loads are supported, providing a stable and scalable power distribution model for the complete multi-board system. To see the Schematic page click here: ["Schematic"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/07-Schematic/Schematic/) or to go to the bill of materials click here: ["BOM"](https://isaiahelixir1.github.io/isaiahelixer1.github.io/05-BOM/BOM/)
