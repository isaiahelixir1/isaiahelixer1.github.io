---
title: Power Budget
--- 

The Environmental Monitoring Subsystem operates entirely at 3.3V.  
All sensors, the PIC microcontroller, LEDs, and communication interfaces require regulated 3.3V.

Estimated worst-case current consumption (including 25% safety margin):

- PIC18F57Q43: ~40 mA
- Gas Sensor (heater + signal): ~150 mA
- I2C Sensors (combined): ~15 mA
- LEDs + Button + Debug: ~10 mA
- Safety Margin (25%): ~50 mA

Estimated Total Required Current: ~265 mA

A 3.3V voltage regulator must therefore supply **at least 300 mA continuously**.

---

## Candidate 1 – MCP1700T-3302E/TT (Linear Regulator)

Manufacturer: Microchip  
Output Current: 250 mA max  
Approx. Cost: ~$0.75  

Pros | Cons
---|---
Very low quiescent current | 250 mA limit (near requirement)
Low dropout voltage | Linear (less efficient)
Simple external circuitry | Limited thermal capability
SOT-23 surface mount | —

---

## Candidate 2 – MCP1825S-3302E/DB (Linear Regulator)

Manufacturer: Microchip  
Output Current: 500 mA max  
Approx. Cost: ~$1.25  

Pros | Cons
---|---
500 mA output (ample headroom) | Slightly higher cost
Low dropout voltage | Linear (heat dissipation if high Vin)
Thermal protection built in | —
Simple capacitor requirements | —
Surface mount package | —

---

## Candidate 3 – TLV75533PDBVR (Texas Instruments)

Output Current: 500 mA  
Approx. Cost: ~$1.00  

Pros | Cons
---|---
500 mA output | Requires careful capacitor selection
Very low dropout | Linear (less efficient than switching)
Low noise output | Not same vendor as MCU
Small SOT-23-5 package | —

---

## Final Selection: MCP1825S-3302E/DB

### Rationale

The MCP1825S-3302E provides:

- 500 mA output capacity (well above 300 mA requirement)
- Built-in thermal and short-circuit protection
- Low dropout voltage for efficient operation
- Simple capacitor requirements
- Surface mount compatibility (EGR 314 compliant)
- Same manufacturer ecosystem as microcontroller

Although the MCP1700 is cheaper, it operates too close to its 250 mA limit and does not provide sufficient safety margin for the gas sensor heater.

The MCP1825S offers strong current headroom, ensuring stable 3.3V regulation under worst-case operating conditions.
