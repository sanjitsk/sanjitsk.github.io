---
layout: page
title: Block Diagram
permalink: /block-diagram/
---

# Embedded Systems Block Diagram
**Author:** Sanjit Selvakumar Kavitha

## Overview  
This page presents the block diagram for the Wi-Fi/Internet Communication Subsystem, featuring debugging LEDs for the MQTT server, RX and TX indicators, a voltage regulator, and upstream/downstream connectors. The entire subsystem is managed by the ESP32-S3-WROOM-N4.

## Block Diagram Preview  
![Block Diagram](./subfolder/blockdiagram_(2).png)

## Download the Block Diagram  
[Draw.io Block Digram View](https://drive.google.com/file/d/1Zu_ZALLJ08QVjuWkUUsSZqnZJdQ2op89/view?usp=sharing)
[Block Diagram PDF Dwonload](https://drive.google.com/file/d/1SzkVRQ6n4QjP0WH3-RH0_l-9RIu3c7Me/view?usp=sharing)

# **Feedback Addressed**

- **Pin Labeling:** All GPIO pins, power rails, headers, and LEDs in the block diagram have been clearly labeled to match the updated Pinout Configuration table.
- **Clarity of Interfaces:** Added UART RX/TX arrows and MQTT publish/subscribe arrows to the server cloud.
- **Subsystem Independence:** Highlighted how the module connects autonomously to Wi-Fi/MQTT and communicates over UART to coordinate with other subsystems.

---

## **Decision-Making & Diagram Development**

1. **System Goals**  
   - **Autonomy:** Must operate independently once powered, without requiring host intervention.  
   - **Bi-directional UART Control:** Receive commands (e.g., “get temperature”) and send processed data to downstream motor and HMI subsystems.  
   - **MQTT Connectivity:** Publish sensor data and receive high-level commands via the cloud.

2. **Diagram Layout**  
   - **Central MCU:** Placed the ESP32 module at the core to emphasize its role as both “brain” (processing + MQTT client) and “bridge” (Wi-Fi ↔ UART).  
   - **Power Domain:** Grouped the DC barrel jack, 3.3 V regulator, and decoupling components on the left to show the power flow.  
   - **Communication Paths:**  
     - **Wi-Fi/MQTT:** Upward arrows to the cloud server, labeled “Publish” (GPIO 40) and “Subscribe” (GPIO 38).  
     - **UART:** Horizontal lines to upstream/downstream headers, indicating RX (GPIO 19/21) and TX (GPIO 20/22).  
   - **Debug Indicators:** TX/RX LEDs and MQTT debug LEDs positioned near their related GPIO pins for easy board population.

3. **Meeting Product Requirements**  
   - **Reliability:** Clear separation of power and data domains ensures minimal interference. Decoupling caps are placed near both the regulator and the ESP32 power pin (3V3).  
   - **Scalability:** Labeled I²C pins and spare GPIOs indicate room for additional sensors or actuators without a redesign.  
   - **Maintainability:** Standardized headers and clear pin labels facilitate firmware updates and hardware debugging. The diagram now directly references table entries, reducing ambiguity for assembly and testing.


