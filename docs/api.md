# MQTT Subsystem UART Communication

## Introduction
The MQTT subsystem is at the end of the UART daisy chain, so my main process will be to pass on the messages I receive back to the beginning of the chain. I will be using the data I receive on the WiFi server, but the MQTT communication will be outlined on a different page than this one. This page will only focus on the UART communications for the smart irrigation system.

## Message Overview
| Message  | Variable Name         | Variable Type | Min Value | Max Value | Example |
|----------|----------------------|--------------|-----------|-----------|---------|
| Message 1 | Soil_Moisture_Value  | uint8_t      | 0         | 255       | 120     |
| Message 2 | Motor_Pump_Value     | uint8_t      | 0         | 255       | 180     |
| Message 3 | Temp_Humidity_Value  | uint8_t      | 0         | 255       | 85      |

I will be receiving three different data values from upstream:
- **Soil moisture sensor data** (from my ESP32).
- **Bi-directional motor pump status** (from a PIC microcontroller).
- **Temperature & humidity sensor data** (from another ESP32).

These values will be uploaded to the WiFi server and then passed along the chain to the LCD display subsystem, ensuring the data is available to all team members. This means that all messages will be both received and transmitted.

## Soil Moisture Value
### Receive Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x03          |
| 3     | Receiver                | uint8_t   | 0x05          |
| 4     | Soil_Moisture_Value     | uint8_t   | Sensor Byte   |
| 5     | End                     | uint8_t   | 0x42          |

### Transmit Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x05          |
| 3     | Receiver                | uint8_t   | 0x04          |
| 4     | Soil_Moisture_Value     | uint8_t   | Sensor Byte   |
| 5     | End                     | uint8_t   | 0x42          |

## Motor Pump Value
### Receive Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x03          |
| 3     | Receiver                | uint8_t   | 0x05          |
| 4     | Motor_Pump_Value        | uint8_t   | Motor Byte    |
| 5     | End                     | uint8_t   | 0x42          |

### Transmit Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x05          |
| 3     | Receiver                | uint8_t   | 0x04          |
| 4     | Motor_Pump_Value        | uint8_t   | Motor Byte    |
| 5     | End                     | uint8_t   | 0x42          |

## Temperature & Humidity Value
### Receive Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x03          |
| 3     | Receiver                | uint8_t   | 0x05          |
| 4     | Temp_Humidity_Value     | uint8_t   | Sensor Byte   |
| 5     | End                     | uint8_t   | 0x42          |

### Transmit Format
| Byte  | Byte Name               | Byte Type | Byte Contents |
|-------|-------------------------|-----------|---------------|
| 1     | Start                   | uint8_t   | 0x41          |
| 2     | Sender                  | uint8_t   | 0x05          |
| 3     | Receiver                | uint8_t   | 0x04          |
| 4     | Temp_Humidity_Value     | uint8_t   | Sensor Byte   |
| 5     | End                     | uint8_t   | 0x42          |

## Summary of the Data Flow in the Daisy Chain
1. **My ESP32** collects soil moisture data and sends it via UART.
2. **The LCD ESP32** displays this data and passes it along.
3. **The PIC (Motor Pump Controller)** receives motor commands and transmits pump status.
4. **The Temperature & Humidity ESP32** sends its sensor data.
5. **The entire chain loops back to me**, ensuring the data is properly cycled through all subsystems.

