---
title: Module's Selected Major Components
---

The following sections describe the selected major components necessary for the Environmental Monitoring Subsystem.  

This subsystem:

- Operates entirely at **3.3V**
- Reads environmental sensors (gas, temperature, humidity, light, pressure)
- Processes data using a **PIC18F57Q43**
- Transmits processed data to teammate boards via a **2×4 ribbon connector**
- Includes debugging (UART), LEDs, and button input

All ICs are surface mount per EGR 314 requirements.

# Power Management

## 3.3V Linear Voltage Regulator  

**MCP1700T-3302E/TT**  
Manufacturer: Microchip  
Approx. Cost: ~$0.75  
[View on DigiKey](https://www.digikey.com/)

Pros | Cons
---|---
Low dropout voltage | Linear (less efficient than switching)
250 mA output current | Limited maximum current
Very low quiescent current | Heat dissipation if input voltage high
Simple external components required | —
SOT-23 surface mount package | —

### Rationale

The entire subsystem operates at 3.3V and requires approximately 200–230 mA worst case (including gas sensor heater and safety margin). The MCP1700 provides sufficient regulated current with low dropout voltage and minimal design complexity. A switching regulator is unnecessary for this current level.

# Microcontroller

## PIC18F57Q43 (TQFP-40)

Manufacturer: Microchip  
Approx. Cost: ~$6–8  
[View on DigiKey](https://www.digikey.com/)

Pros | Cons
---|---
40 pins (ample GPIO) | 8-bit architecture
12-bit ADC (ideal for gas sensor) | No built-in wireless
Multiple I2C peripherals | Less RAM than ESP32
Multiple UART peripherals | —
Fully supported in MPLabX Melody | —
Surface mount TQFP package | —
Compatible with course ecosystem | —

### Rationale

The PIC18F57Q43 provides sufficient:

- ADC channels (gas sensor)
- I2C bus (environmental sensors)
- UART (debug header)
- GPIO (LEDs + button)
- ICSP programming interface

It avoids the soldering difficulty and complexity of QFN-based ESP32 devices while fully meeting subsystem requirements.

# Sensor – Gas Sensor

## MQ-135 (Analog Gas Sensor, SMD Variant)

Manufacturer: Hanwei  
Approx. Cost: ~$5–10  

Pros | Cons
---|---
Analog output (direct ADC interface) | Requires calibration
Simple hardware interface | Heater consumes significant current
Well-documented behavior | Large footprint
No complex initialization | —

### Rationale

The MQ-135 provides an analog voltage proportional to gas concentration. This connects directly to the PIC's 12-bit ADC (RA0/AN0). This simplifies firmware compared to digital gas sensors requiring I2C configuration and calibration routines.

# Sensor – Temperature & Humidity

## SHT31 (I2C Digital Sensor)

Manufacturer: Sensirion  
Approx. Cost: ~$6  

Pros | Cons
---|---
High accuracy | Higher cost than basic sensors
I2C interface (shared bus) | Requires I2C library
Fully surface mount | —
Industry standard | —

### Rationale

The SHT31 communicates over I2C (RC3/RC4) and shares the bus with other digital sensors. It provides reliable environmental measurements with minimal GPIO usage (2 signal pins).

# Sensor – Light Intensity

## BH1750 (I2C Light Sensor)

Manufacturer: ROHM  
Approx. Cost: ~$3  

Pros | Cons
---|---
Digital I2C output | Requires library support
High resolution | —
Low current consumption | —
Simple interface | —

### Rationale

The BH1750 eliminates the need for additional ADC channels and provides accurate digital light intensity readings via the shared I2C bus.

# Sensor – Barometric Pressure

## BMP280 (I2C Pressure Sensor)

Manufacturer: Bosch  
Approx. Cost: ~$4  

Pros | Cons
---|---
I2C digital interface | Requires initialization sequence
Small SMD package | —
Low power consumption | —
Widely supported | —

### Rationale

The BMP280 integrates easily into the shared I2C bus and provides stable barometric pressure measurements while minimizing required MCU pins.

# Summary of Final Selected Components

Subsystem | Selected Component
---|---
Voltage Regulation | MCP1700T-3302E/TT
Microcontroller | PIC18F57Q43 (TQFP-40)
Gas Sensor | MQ-135 (Analog)
Temperature/Humidity | SHT31 (I2C)
Light Sensor | BH1750 (I2C)
Pressure Sensor | BMP280 (I2C)

All selected components:

- Operate at 3.3V
- Are surface mount compatible
- Have complete datasheets
- Are compatible with PIC18F57Q43 peripherals
- Meet EGR 314 subsystem requirements
