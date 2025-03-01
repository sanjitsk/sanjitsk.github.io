---
title: Component Selection
tags:
  - tag1
  - tag2
---

# Wi-Fi-Enabled Data Collection and Transmission Subsystem

## **Overview**

In this embedded systems design project, we focus on building a subsystem capable of collecting sensor data, transmitting it over a Wi-Fi network managed by the ESP32 microcontroller, and updating a real-time webpage hosted on GitHub. This document provides an in-depth look into the component selection process, emphasizing efficient power regulation, dependable wireless communication, and seamless sensor integration.

---

## **Component Selection**

### **Microcontroller**

For the heart of our subsystem, we’ve chosen the **ESP32 microcontroller**. Its built-in Wi-Fi connectivity, processing capabilities, and flexibility make it the perfect choice for our project, handling both sensor data acquisition and transmission efficiently.

| **Option**               | **Advantages**                                                | **Disadvantages**                                     | **Cost & Link**                                                                      |
|--------------------------|---------------------------------------------------------------|------------------------------------------------------|--------------------------------------------------------------------------------------|
| **ESP32-S3-WROOM-1-N4**  | Built-in Wi-Fi/Bluetooth, supports I2C/SPI/UART, low power modes, 4MB Flash | 3.3V logic may require level shifters for certain peripherals | [$2.95 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639) |
| ESP8266                  | Affordable, simple to use                                     | Limited GPIO pins, lacks dual-core processor         | [$1.60 DigiKey](https://www.digikey.com/en/products/detail/espressif-systems/ESP8266EX/8028401) |
| Raspberry Pi Pico W      | Dual-core, built-in Wi-Fi                                    | Higher power usage, larger physical size             | [$6.00 DigiKey](https://www.digikey.com/en/products/detail/raspberry-pi/SC0918/16627943) |

**Choice:**  
We selected the **ESP32-S3-WROOM-1-N4** due to its superior Wi-Fi capabilities, dual-core processor for multitasking, and compatibility with sensor interfaces like I2C and SPI. Its low power consumption and comprehensive library support make it ideal for real-time data processing and transmission.

![ESP32-S3-WROOM-1-N4](./subfolder/esp32.png)

---

### **Power Regulation**

To ensure stable operation of the ESP32 and connected sensors, we require a voltage regulator that steps down the input voltage to a steady 3.3V.

| **Option**           | **Advantages**                                       | **Disadvantages**                        | **Cost & Link**                                                                  |
|----------------------|------------------------------------------------------|----------------------------------------|----------------------------------------------------------------------------------|
| **AMS1117-3.3**      | Simple design, low cost                              | Low efficiency                         | [$0.68 DigiKey](https://www.digikey.com/en/products/detail/umw/AMS1117-3-3/17635254) |
| LM2596               | High efficiency, more robust                        | Larger size                            | [$6.70 DigiKey](https://www.digikey.com/en/products/detail/texas-instruments/LM2596S-ADJ-NOPB/363705) |
| HT7333               | Ultra-low quiescent current                          | Limited current output                 | [$0.65 DigiKey](https://www.digikey.com/en/products/detail/umw/HT7333-A/17635230) |

**Choice:**  
The **AMS1117-3.3** was chosen for its simplicity, affordability, and compatibility with surface-mount applications. It adequately meets the power needs of the ESP32 and associated peripherals.

![AMS1117-3.3](./subfolder/vregulator.png)

---

### **Power Input**

To provide consistent power for the ESP32 and sensors, we need a reliable input source.

**Choice:**  
A **DC Barrel Jack Adapter** was chosen for its reliable performance, ease of connection, and ability to supply stable power from an external adapter, ensuring uninterrupted subsystem operation.

---

## **Additional Recommended Components**

For enhanced functionality and robustness, the following components are recommended:

1. **Boot and Enable Buttons:**  
   - Add pull-up resistors with Boot and Enable buttons for easier troubleshooting.
   
2. **Capacitors (Decoupling):**  
   - 10µF and 0.1µF capacitors to stabilize voltage and reduce noise near the ESP32 and voltage regulator.

3. **Level Shifters:**  
   - Ensure compatibility between sensors operating at 5V logic and the ESP32's 3.3V GPIO pins.

4. **LED Indicators:**  
   - Incorporate LEDs to monitor power status and Wi-Fi activity for better debugging.

---

## **Subsystem Responsibilities**

The key tasks for this subsystem include:

1. **Data Collection:**  
   - Interface with sensors using I2C, SPI, or UART protocols.
   
2. **Wi-Fi Communication:**  
   - Use the ESP32's built-in Wi-Fi to create a local network and transmit data.

3. **Power Management:**  
   - Regulate voltage with the AMS1117-3.3 to maintain a stable 3.3V supply.

4. **Data Transmission:**  
   - Update sensor readings on a real-time OLED display.

5. **Integration:**  
   - Ensure seamless compatibility between the sensors, motors, and microcontroller using correct pin assignments and libraries.

---

## **Pin Assignments**

### ESP32 Pin Configuration:

| **Peripheral**       | **Pin Assignment**   |
|----------------------|----------------------|
| I2C SDA              | GPIO20               |
| I2C SCL              | GPIO21               |
| SPI MOSI             | GPIO24               |
| SPI MISO             | GPIO19               |
| SPI SCLK             | GPIO18               |
| UART TX/RX           | Debugging Pins       |
| Power Input          | VIN (via DC Barrel Jack) |

---

## **Conclusion**

The selected components provide a reliable and efficient design for the data collection and transmission subsystem. By incorporating proper power regulation, dependable Wi-Fi communication, and necessary peripheral enhancements, this subsystem is well-suited for continuous, real-time operation in our embedded systems project.

---
