# nRF52840 Sensor Connection Diagram

This document outlines the wiring and pin assignments for connecting various environmental sensors to the nRF52840 development board. The goal is to monitor soil moisture (capacitive and resistive), air temperature/humidity/pressure, and ambient light.

## Pin Assignments

- **P1.06** → Analog Output of HW-101 Capacitive Soil Moisture Sensor
- **P1.04** → Analog Output of Resistive Soil Moisture (EC) Sensor
- **P1.00 (SDA)** → I2C Data Line shared between BME280 and GY-302
- **P0.11 (SCL)** → I2C Clock Line shared between BME280 and GY-302

## Power Connections

All components (sensors and the nRF52840 board) are powered via the **3.3V (3V3)** power rail and share a common **GND** (ground) line.

## Components Overview

- **HW-101 Capacitive Soil Moisture Sensor**: Measures soil moisture without direct exposure of the electrodes to soil, providing better durability and less corrosion.
- **Resistive Humidity (EC) Soil Sensor**: Provides analog output proportional to soil conductivity (moisture level), but is prone to corrosion over time.
- **BME280**: A digital sensor that measures temperature, humidity, and barometric pressure over I2C.
- **GY-302**: A digital light sensor (BH1750) using I2C for ambient light measurements.

---

## Wiring Diagram

Below is a visual representation of the connections:

```mermaid
flowchart TD
    NRF[nRF52840]
    HW_101[HW-101 Cap Hum Soil Sensor]
    RHS["Resistive Hum Soil Sensor (EC)"]
    BME280[BME280 Sensor]
    GY302[GY-302 Lux Sensor]
    SDA_SPLIT(( ))
    SCL_SPLIT(( ))
    PWR33[3V3 Power]
    GND[Ground]

    NRF -- P1.06 (AO) --- HW_101
    NRF -- P1.04 (AO) --- RHS
    NRF -- P1.00 (SDA) --> SDA_SPLIT
    SDA_SPLIT --> BME280
    SDA_SPLIT --> GY302
    NRF -- P0.11 (SCL) --> SCL_SPLIT
    SCL_SPLIT --> BME280
    SCL_SPLIT --> GY302

    HW_101 -.- PWR33
    RHS -.- PWR33
    BME280 -.- PWR33
    GY302 -.- PWR33
    NRF -.- PWR33
    
    HW_101 -.- GND
    RHS -.- GND
    BME280 -.- GND
    GY302 -.- GND
    NRF -.- GND

    style PWR33 fill:#ffcccc,stroke:#ff0000,stroke-width:2px
    style GND fill:#e0e0e0,stroke:#000000,stroke-width:2px
```
