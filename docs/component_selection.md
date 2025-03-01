---
title: Component Selection
tags:
- tag1
- tag2
---

# Subsystem Documentation: Wi-Fi-Enabled Data Collection and Transmission

## **Introduction**
As part of our embedded systems design project, this subsystem is responsible for acquiring sensor data, transmitting it over a Wi-Fi network established by the ESP32 microcontroller, and updating a GitHub-hosted webpage in real time. This document details the component selection process, focusing on efficient power regulation, reliable wireless communication, and seamless sensor integration.


---

## **Major Component Selections**

### **Microcontroller Selection**
The ESP32 microcontroller serves as the core of this subsystem, offering built-in Wi-Fi connectivity and the processing power necessary for sensor data acquisition and transmission.

| **Option**               | **Pros**                                                                 | **Cons**                                                       | **Unit Cost & Link**                                                                 |
|---------------------------|-------------------------------------------------------------------------|----------------------------------------------------------------|-------------------------------------------------------------------------------------|
| **ESP32-S3-WROOM-1-N4 (Final Choice)**  | Built-in Wi-Fi/Bluetooth, supports I2C/SPI/UART, low power modes, 4MB Flash | 3.3V logic may require level shifters for some peripherals      | [$2.95 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639) |
| ESP8266                  | Low cost, simple to use                                                 | Limited GPIO pins, no dual-core processor                      | [$1.60 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP8266EX/8028401) |
| Raspberry Pi Pico W      | Dual-core processor, Wi-Fi support                                      | Higher power consumption, larger physical size                 | [$6.00 DigiKey](https://www.digikey.com/en/products/detail/raspberry-pi/SC0918/16627943) |

**Final Selection:**
![ESP32-S3-WROOM-1-N4](./subfolder/esp32.png)
The ESP32-S3-WROOM-1-N4 was chosen for its robust Wi-Fi capabilities, dual-core processor for multitasking, and compatibility with I2C/SPI interfaces required for sensor integration. Its low power consumption and extensive library support make it ideal for real-time data processing and transmission.

---

### **Power Regulation**
A voltage regulator is required to step down the input voltage to 3.3V, ensuring stable operation for the ESP32 and connected sensors.

| **Option**           | **Pros**                                                  | **Cons**                                   | **Unit Cost & Link**                                                                 |
|-----------------------|----------------------------------------------------------|-------------------------------------------|-------------------------------------------------------------------------------------|
| **AMS1117-3.3 (Final Choice)**         | Simple design                                            | Low efficiency                            | [$0.68 DigiKey](https://www.digikey.com/en/products/detail/umw/AMS1117-3-3/17635254) |
| LM2596                | High efficiency                                          | Larger physical size                      | [$6.70 DigiKey](https://www.digikey.com/en/products/detail/texas-instruments/LM2596S-ADJ-NOPB/363705) |
| HT7333    | Ultra-low quiescent current                              | Limited current output                    | [$0.65 DigiKey](https://www.digikey.com/en/products/detail/umw/HT7333-A/17635230) |

**Final Selection:**
![AMS1117-3.3](./subfolder/vregulator.png)
The AMS1117-3.3 was chosen for its low cost, ease of implementation, and compatibility with surface-mount applications. It efficiently meets the current requirements of the ESP32 and connected peripherals.

---

### **Power Input**
The subsystem requires a reliable power source capable of providing sufficient current for the ESP32 and sensors.


**Final Selection: DC Barrel Jack Adapter**  
A DC barrel jack adapter was selected due to its reliability, ease of connection, and ability to deliver consistent power from an external adapter, ensuring uninterrupted operation.



---

## **Additional Components to Enhance Subsystem**
To improve functionality and robustness, the following additional components are recommended:

1. **Boot and Enable Buttons**
   - Include Boot and Enable buttons with a pull up resistor.

2. **Capacitors (Decoupling):**
   - Add 10µF and 0.1µF capacitors near the ESP32 and voltage regulator to reduce noise and stabilize voltage.
   
3. **Level Shifters:**
   - Ensure compatibility between sensors operating at 5V logic levels and the ESP32's 3.3V GPIO pins.

4. **LED Indicators:**
   - Include LEDs for power status and Wi-Fi activity to aid debugging.


---

## **Responsibilities for the subsystem**

1. **Data Collection:** Interface with all sensors in the system using I2C/SPI/UART protocols.
2. **Wi-Fi Communication:** Use the ESP32's built-in Wi-Fi module to create a local network and transmit sensor data.
3. **Power Management:** Regulate input voltage using an AMS1117-3.3 regulator to ensure a stable 3.3V supply.
4. **Data Transmission:** Continuously update sensor readings on a the OLED display
5. **Integration:** Ensure seamless compatibility between sensors, motors and the microcontroller through proper pin allocation and library selection.

---

## **Pin Allocation**

### ESP32 Peripheral Pin Assignments:
| Peripheral      | Pin Assignment       |
|------------------|----------------------|
| I2C SDA          | GPIO20              |
| I2C SCL          | GPIO21             |
| SPI MOSI         | GPIO24              |
| SPI MISO         | GPIO19              |
| SPI SCLK         | GPIO18              |
| UART TX/RX       | Debugging Pins      |
| Power Input      | VIN via DC Barrel Jack |

---

## **Conclusion**

The selected components ensure efficient sensor integration with the ESP32 microcontroller while meeting system requirements for real-time data acquisition and wireless transmission. By incorporating proper power regulation, reliable Wi-Fi communication, and auxiliary enhancements, this subsystem is optimized for robust and continuous operation.
