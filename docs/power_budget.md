# Power Budget Calculation

## Power Formula Used
\[
P = V \times I
\]
where:  
- **P** = Power in watts (W)  
- **V** = Voltage in volts (V)  
- **I** = Current in amperes (A)  

---

## Updated Power Budget Table

| **Component**                                        | **Qty** | **Voltage (V)** | **Current (A) per unit** | **Power (W) per unit** | **Total Power (W)** | **Notes**                                                                                  |
|------------------------------------------------------|:-------:|:---------------:|:------------------------:|:----------------------:|:-------------------:|--------------------------------------------------------------------------------------------|
| **ESP32-S3-WROOM-1-N4**                              |    1    |      3.3 V      |         0.150 A          |   3.3 V × 0.150 A = 0.495 W   |       0.495 W       | Typical operating current; peaks can be higher but averaged here.                         |
| **Green LEDs (Chip LED + 220 Ω series resistor)**    |    5    |      3.3 V      |   (3.3−2.2)V/220Ω ≈ 0.005 A   | 3.3 V × 0.005 A = 0.0165 W   | 5 × 0.0165 = 0.0825 W | Five LEDs D3–D7 with 220 Ω resistors (R4,R6,R7,R9,R11).                                   |
| **Green LEDs (Chip LED + 470 Ω series resistor)**    |    2    |      3.3 V      | (3.3−2.2)V/470Ω ≈ 0.0023 A  | 3.3 V × 0.0023 A = 0.0076 W  | 2 × 0.0076 = 0.0152 W | Two LEDs D5,D6 with 470 Ω resistors (R5,R10).                                              |
| **Pull-up / general resistors (10 kΩ)**              |    4    |      3.3 V      |       3.3 V/10 kΩ = 0.00033 A     | 3.3 V × 0.00033 A = 0.0011 W | 4 × 0.0011 = 0.0044 W | Boot/Enable pull-ups and other logic pull-ups (R1,R2,R3,R8).                                |
| **Schottky Power Diodes (SOD-323)**                  |    2    |      ~0.3 V drop|         0.150 A          | 0.3 V × 0.150 A = 0.045 W   | 2 × 0.045 = 0.090 W  | D1,D2 – estimate at full ESP32 current path.                                              |
| **AP63203WU-7 Buck Converter (efficiency ~85%)**     |    1    |  Input 5 V → 3.3 V |  I_out ≈ 0.157 A (sum of all loads) | P_out ≈ 3.3 V × 0.157 A = 0.518 W | P_in ≈ 0.518 W/0.85 = 0.610 W | Accounts for total rail load (ESP32 + LEDs + resistors + diodes) and converter losses.   |
| **Fuse, connectors, switches, caps, inductor, USB**  |  – (var)  |       –         |           –              |          –             |        ~0 W         | Passive components; negligible steady-state draw.                                         |

---

## Total Estimated Power Budget

Summing all “Total Power (W)” entries:

\[
0.495 + 0.0825 + 0.0152 + 0.0044 + 0.090 + 0.610 \approx \mathbf{1.30\;W}
\]

This accounts for  
- the ESP32’s average consumption,  
- five high-current LEDs and two low-current LEDs,  
- pull-up resistors,  
- Schottky diode drops, and  
- buck-converter inefficiency.  

At a 5 V input, the converter will draw approximately  
\[
I_{\text{in}} = \frac{0.610\ \text{W}}{5\ \text{V}} = 0.122\ \text{A}
\]
from the source, for a total input-side power of ~0.61 W.

---

**Note:** Peak currents (e.g. Wi-Fi bursts on the ESP32) may briefly exceed these averages; ensure the power supply and regulator heat-sink margins accommodate short-term surges.
