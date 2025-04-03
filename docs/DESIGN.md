# Design Document

## Overview

This document outlines the architectural and design decisions for the EdenSync sensor hardware. This is done in mind with the highly likely usage of zephyr SDK for development.

## High-Level Architecture

### System Overview

EdenSync is a smart plant monitoring system designed to integrate seamlessly with Home Assistant and also be viewable as web app. The system consists of sensor nodes that collect environmental data and transmit it through a central bridge to the cloud.

### Technology Stack

- **Microcontroller**: NRF52 Series (chosen for Thread support and low-power, reliable communication).
- **Sensors**: Various environmental sensors for plant monitoring.

## Key Design Decisions

### 1. **Soil Moisture Sensor: Capacitive Sensor**

- **Context:** Measuring soil moisture is critical for determining plant hydration needs.
- **Decision:** The The HW-101 Capacitive Soil Moisture Sensor was chosen over resistive sensors ([Datasheet](https://www.datocms-assets.com/28969/1662716326-hw-101-hw-moisture-sensor-v1-0.pdf)), it works with analog output.
- **Alternatives Considered:** Resistive moisture sensors.
- **Trade-offs:**
  - **Pros:** No corrosion issues, longer lifespan, more accurate.
  - **Cons:** Slightly more expensive than resistive sensors.
- **Cost:** 1-3 euros.

### 2. **Temperature Sensor & Environmental Humidity Sensor**

- **Context:** Temperature and humidity sensor is chosen to be combined to limit the components that needs exposure outside of the enclosure, it will also keep the cost down.
- **Decision:** The BME280 is chosen since seemed to work best out of [this test](https://www.kandrsmith.org/RJS/Misc/Hygrometers/calib_many.html). And it has support in zephyr already.
- **Alternatives Considered:** [A comparison list](https://99tech.com.au/sensor-information/dht-vs-sht-vs-ds18b20-vs-lm35/)
- **Trade-offs:**
  - **Pros:** Higher accuracy according to the test, digital output reduces noise, has a pressure sensor in case we want to incorporate that.
  - **Cons:** Requires slightly more power than analog sensors. Does't have a housing so will need to be coated/epoxied.
- **Cost:** 1-2 euros.
-

### 3. **Soil pH Sensor (Optional)**

- **Context:** Measuring soil pH helps assess nutrient availability and soil health. However, pH changes occur gradually, so daily monitoring may not be necessary.
- **Decision:** Due to cost and maintenance concerns, a dedicated pH sensor is not included in the standard setup. If needed, periodic measurements using a handheld pH meter or chemical test kit are recommended.
- **Alternatives Considered:**  
  - **Electronic pH sensors** (e.g., [PH4502C module](https://nl.aliexpress.com/item/1005005732537764.html))  
- **Trade-offs:**  
  - **Pros:** Provides valuable soil health insights, useful for optimizing plant growth  
  - **Cons:** Expensive, requires frequent calibration, probes degrade over time, not necessary for daily monitoring  
- **Cost:** €13-14 for an electronic pH sensor.

### 4. **Soil Electrical Conductivity (EC) Sensor**

- **Context:** Electrical conductivity (EC) is used to measure the ion concentration in the soil, which correlates with nutrient availability and salinity levels. Instead of a standard antenna, carbon rods or stainless steel electrodes will be used for improved durability and accuracy.
- **Decision:** A moisture resistance detector module ([AliExpress link](https://nl.aliexpress.com/item/1005008638383068.html)) will be repurposed for EC measurement. By replacing its original antenna with carbon/stainless steel rods and calibrating it manually,
- EC values can be derived from resistance readings. The relationship between resistance and EC will be established through calibration against known solutions.
- **Alternatives Considered:** Dedicated EC probes, Custom AC measurement circuits (using an Arduino and operational amplifiers for excitation and signal processing).
- **Trade-offs:**
  - **Pros:** Cost-effective solution, requires no additional circuitry.
  - **Cons:** Because of DC excitation, ionization will happen. This will have to be kept to minimum time. Conversion from resistance to EC is not straightforward and will require calibration. Moisture content will also affect the readings.
- **Cost:** 1-2 euros + 2 euros for rods.

### 5. **Light Intensity (LUX) Sensor**

- **Context:** Monitoring light intensity helps determine the exposure of plants to light.
- **Decision:** The GY-302/BH1750FVI or GY-30/BH1750  light sensors can be used. The GY-302 is the smallest form factor but lacks a protection diode on the clock line and voltage conversion on the I2C pins. These modules cost between 1 and 2 euros. It will be placed at the top of the enclosure with a transparent cover. `TODO: Choose between these`
- **Alternatives Considered:** TEMT6000, a PAR sensor (Photosynthetically Active Radiation, for more accurate light measurement).
- **Trade-offs:**
  - **Pros:** Has already a zephyr implementation. Cost-effective.
  - **Cons:** TBD.
