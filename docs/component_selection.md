---
title: Component Selection
tags:
- tag1
- tag2
---

# Subsystem Documentation: Wi-Fi-Enabled Data Collection and Transmission

## **Introduction**
As part of our embedded systems design project, my subsystem is responsible for collecting data from all sensors in the system, transmitting it via a Wi-Fi network created by the ESP32 microcontroller, and updating this data in real-time on a GitHub-hosted webpage. This document outlines the selection of components for my subsystem, focusing on efficient power regulation, reliable wireless communication, and seamless integration with sensors.

---

## **Major Component Selections**

### **Microcontroller Selection**
The ESP32 microcontroller is the core of this subsystem, providing Wi-Fi connectivity and the processing power needed to gather and transmit sensor data.

| **Option**               | **Pros**                                                                 | **Cons**                                                       | **Unit Cost & Link**                                                                 |
|---------------------------|-------------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------|
| **ESP32-S3-WROOM-1-N4 (Final Choice)**  | Built-in Wi-Fi/Bluetooth, supports I2C/SPI/UART, low power modes, 4MB Flash | 3.3V logic may require level shifters for some peripherals      | [$2.95 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639) |
| ESP8266                  | Low cost, simple to use                                                 | Limited GPIO pins, no dual-core processor                      | [$1.60 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP8266EX/8028401) |
| Raspberry Pi Pico W      | Dual-core processor, Wi-Fi support                                      | Higher power consumption, larger physical size                 | [$6.00 DigiKey](https://www.digikey.com/en/products/detail/raspberry-pi/SC0918/16627943) |

**Final Selection: ESP32-S3-WROOM-1-N4**  
*Rationale:* The ESP32-S3-WROOM-1-N4 was selected due to its robust Wi-Fi capabilities, dual-core processor for multitasking, and compatibility with I2C/SPI protocols required for sensor interfacing. Its low power consumption and extensive library support make it ideal for real-time data transmission.

---

### **Power Regulation**
To ensure stable operation of the ESP32 and sensors, a voltage regulator is required to step down the input voltage to 3.3V.

| **Option**           | **Pros**                                                  | **Cons**                                   | **Unit Cost & Link**                                                                 |
|-----------------------|----------------------------------------------------------|-------------------------------------------|-------------------------------------------------------------------------------------|
| **AMS1117-3.3 (Final Choice)**         | Simple design                                            | Low efficiency                            | [$0.68 DigiKey](https://www.digikey.com/en/products/detail/umw/AMS1117-3-3/17635254) |
| LM2596                | High efficiency                                          | Larger physical size                      | [$6.70 DigiKey](https://www.digikey.com/en/products/detail/texas-instruments/LM2596S-ADJ-NOPB/363705) |
| HT7333    | Ultra-low quiescent current                              | Limited current output                    | [$0.65 DigiKey](https://www.digikey.com/en/products/detail/umw/HT7333-A/17635230) |

**Final Selection: HT7333**  
*Rationale:* The AMS1117-3.3 was selected as the optimal voltage regulator for this subsystem due to its low cost, simplicity, compatibility with surface-mount requirements, and ability to meet the current demands of the ESP32 microcontroller and connected sensors.

---

### **Power Input**
The subsystem requires a reliable power source capable of providing sufficient current for the ESP32 and sensors.


**Final Selection: DC Barrel Jack Adapter**  
*Rationale:* A DC barrel jack adapter was selected for its simplicity and ability to provide stable power from a wall adapter or external supply. This ensures consistent operation during extended use.

---

## **Additional Components to Enhance Subsystem**
To improve functionality and robustness, the following additional components are recommended:

1. **Capacitors (Decoupling):**
   - Add 10µF and 0.1µF capacitors near the ESP32 and voltage regulator to reduce noise and stabilize voltage.
   
2. **Level Shifters:**
   - Ensure compatibility between sensors operating at 5V logic levels and the ESP32's 3.3V GPIO pins.

3. **Heat Sink (Optional):**
   - Improve thermal performance of the voltage regulator during extended operation.

4. **LED Indicators:**
   - Include LEDs for power status and Wi-Fi activity to aid debugging.

---

## **Responsibilities**

1. **Data Collection:** Interface with all sensors in the system using I2C/SPI/UART protocols.
2. **Wi-Fi Communication:** Use the ESP32's built-in Wi-Fi module to create a local network and transmit sensor data.
3. **Power Management:** Regulate input voltage using an AMS1117-3.3 regulator to ensure a stable 3.3V supply.
4. **Data Transmission:** Continuously update sensor readings on a GitHub-hosted webpage in real time.
5. **Integration:** Ensure seamless compatibility between sensors and the microcontroller through proper pin allocation and library selection.

---

## **Pin Allocation**

### ESP32 Peripheral Pin Assignments:
| Peripheral      | Pin Assignment       |
|------------------|----------------------|
| I2C SDA          | GPIO21              |
| I2C SCL          | GPIO22              |
| SPI MOSI         | GPIO23              |
| SPI MISO         | GPIO19              |
| SPI SCLK         | GPIO18              |
| UART TX/RX       | Debugging Pins      |
| Power Input      | VIN via DC Barrel Jack |

---

## **Conclusion**

The selected components ensure efficient integration of all sensors with the ESP32 microcontroller while meeting project specifications for real-time data collection and transmission to a GitHub-hosted webpage.
