# Design Document

## Overview

This document outlines the architectural and design decisions for the EdenSync sensor hardware. This is done in mind with the highly likely usage of zephyr sdk for development.

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

### 4. **Soil Electrical Conductivity (EC) Sensor**

TBD

### 5. **Light Intensity (LUX) Sensor**

- **Context:** Monitoring light intensity helps determine the exposure of plants to light.
- **Decision:** The GY-302/BH1750FVI or GY-30/BH1750  light sensors can be used. The GY-302 is the smallest form factor but lacks a protection diode on the clock line and voltage conversion on the I2C pins. These modules cost between 1 and 2 euros. It will be placed at the top of the enclosure with a transparent cover. `TODO: Choose between these`
- **Alternatives Considered:** TEMT6000
- **Trade-offs:**
  - **Pros:** Has already a zephyr implementation.
  - **Cons:** TBD.
