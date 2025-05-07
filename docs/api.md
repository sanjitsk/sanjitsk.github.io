## Firmware API & Behavior

This firmware on the ESP32 sits at the end of our UART daisy‐chain (“T” node) and also acts as an MQTT client.  Its two main tasks are:

1. **Periodic Temperature Request**  
   - **Every 10 seconds**, send an 8-byte UART frame to ask the PIC (“E” node) for the latest temperature reading.  
   - **Frame:**  
     ```
     Byte  Content     ASCII
      0    'F'         0x46
      1    'S'         0x53
      2    'T'         sender = 'T' (this ESP32)
      3    'E'         receiver = 'E' (the PIC)
      4    '0'         payload high nibble ('0')
      5    '0'         payload low nibble ('0')
      6    'F'         0x46
      7    'S'         0x53
     ```  
   - **Constant in code:**  
     ```python
     CMD_REQ = b'FSTE00FS'
     ```

2. **UART Listener & MQTT Publisher**  
   - **Listens** for any **8-byte** frame addressed back to this node (receiver = 'T').  
   - **Validates** header/footer (`b'FS' ... b'FS'`).  
   - **Parses** the two ASCII-digit payload bytes into an integer **temperature** (0–99 °C).  
   - **Publishes** the parsed temperature to the MQTT topic `test/sanjit/SUB`.  
   - **Decides** on a motor command based on temperature:  
     - `≤ 20 °C` → **REV**  
     - `21–34 °C` → **OFF**  
     - `≥ 35 °C` → **ON**  
   - **Sends** the corresponding 8-byte “motor control” frame back over UART (to the PIC, receiver = 'S').  
   - **Publishes** the motor state string (`b'REV'`, `b'OFF'`, or `b'ON'`) to MQTT topic `test/sanjit/PUB`.

### UART ↔ MQTT Mapping

| UART Frame       | Meaning            | MQTT Publish            | Topic                   |
|------------------|--------------------|--------------------------|-------------------------|
| `FSTE00FS`       | Ask PIC for temp   | (none)                   | —                       |
| `FS T→T temp FS` | Temperature reply  | `str(temp).encode()`     | `test/sanjit/SUB`       |
| `FS T→S 03 FS`   | Motor = REV        | `b'REV'`                 | `test/sanjit/PUB`       |
| `FS T→S 01 FS`   | Motor = OFF        | `b'OFF'`                 | `test/sanjit/PUB`       |
| `FS T→S 02 FS`   | Motor = ON         | `b'ON'`                  | `test/sanjit/PUB`       |

> **Note on Topics**  
> - **SUB** topic (**`test/sanjit/SUB`**) carries only temperature strings.  
> - **PUB** topic (**`test/sanjit/PUB`**) carries motor‐state commands.

### Helper Functions in `main.py`

```python
def uart_send(frame: bytes):
    """Write an 8-byte frame to UART and log it."""
    uart.write(frame)

async def publish(topic: bytes, msg: bytes):
    """Publish msg to MQTT topic with QoS=1 and log it."""
    await client.publish(topic, msg, qos=1)
