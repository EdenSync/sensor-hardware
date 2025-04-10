# EdenSync Sensor Hardware

## Overview

EdenSync is a smart plant monitoring system designed to integrate seamlessly with Home Assistant and other smart home platforms. This repository focuses on the hardware components of the system, including sensor nodes and the central bridge.

## Hardware Architecture

### Sensor Nodes

Each sensor node is responsible for collecting environmental data and transmitting it to the bridge.

#### **Microcontroller**

- **NRF52 Series**: Chosen for Thread support, providing low-power, reliable communication.

#### **Sensors**

For details on the sensor choices and implementation, see the [Design Document](docs/DESIGN.md).

#### **Power Options**

- Rechargeable lithium-ion battery
- Solar panel integration for outdoor use

### Communication Protocols

- **Thread**: Secure, low-power, and scalable mesh networking protocol.
  - Chosen for its efficiency and support for IoT devices.
- **Bridge Communication**: The bridge will be an **ESP Thread Border Router**, which provides both Thread and Wi-Fi connectivity.

### Bridge Hardware

- **Processor**: ESP-based Thread Border Router to handle Thread communication and Wi-Fi connectivity.
- **Communication Module**: Thread radio for reliable data transfer between the sensor nodes and the bridge.
- **Connectivity**: Wi-Fi for integration with Home Assistant and cloud services.

### Cloud-Based Processing

- All processing will take place on the cloud, enabling scalable data analytics, storage, and integration with Home Assistant.

## Integration

- **MQTT** is used for lightweight and efficient communication.
- **Home Assistant**: Full compatibility for automation and monitoring of plant health.
- **Web Dashboard**: For remote monitoring, alerts, and control.

## Key Considerations

- **Energy Efficiency**: Low-power components to maximize battery life.
- **Ease of Setup**: Designed for easy, plug-and-play installation.
- **Scalability**: Supports adding more sensor nodes as needed, with future expansion potential.

---

For firmware and software details, refer to the respective repositories in the EdenSync ecosystem.
