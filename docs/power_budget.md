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

| **Component**                                        | **Qty** | **Voltage (V)** | **Current (A) per unit** | **Power (W) per unit**     | **Total Power (W)**   | **Notes**                                                                                  |
|------------------------------------------------------|:-------:|:---------------:|:------------------------:|:--------------------------:|:---------------------:|--------------------------------------------------------------------------------------------|
| **ESP32-S3-WROOM-1-N4**                              |    1    |      3.3 V      |         0.150 A          | 3.3 V × 0.150 A = 0.495 W    |       0.495 W         | Typical operating current; peaks can be higher but averaged here.                         |
| **Green LEDs (+ 220 Ω series resistor)**             |    5    |      3.3 V      | (3.3–2.2)V/220 Ω ≈ 0.005 A | 3.3 V × 0.005 A = 0.0165 W   | 5 × 0.0165 = 0.0825 W  | Five LEDs D3–D7 with 220 Ω resistors (R4,R6,R7,R9,R11).                                   |
| **Green LEDs (+ 470 Ω series resistor)**             |    2    |      3.3 V      | (3.3–2.2)V/470 Ω ≈ 0.0023 A| 3.3 V × 0.0023 A = 0.0076 W  | 2 × 0.0076 = 0.0152 W  | Two LEDs D5,D6 with 470 Ω resistors (R5,R10).                                              |
| **Pull-up / general resistors (10 kΩ)**              |    4    |      3.3 V      | 3.3 V/10 kΩ = 0.00033 A    | 3.3 V × 0.00033 A = 0.0011 W | 4 × 0.0011 = 0.0044 W  | Boot/Enable pull-ups and other logic pull-ups (R1,R2,R3,R8).                                |
| **Schottky Power Diodes (SOD-323)**                  |    2    |      ~0.3 V     |         0.150 A          | 0.3 V × 0.150 A = 0.045 W    | 2 × 0.045 = 0.090 W    | D1,D2 – estimate at full ESP32 current path.                                              |
| **AP63203WU-7 Buck Converter (≈ 85 % eff.)**         |    1    | 5 V → 3.3 V     | I_out ≈ 0.157 A           | P_out ≈ 3.3 V × 0.157 A = 0.518 W | P_in ≈ 0.518 W/0.85 = 0.610 W | Accounts for total rail load (ESP32 + LEDs + resistors + diodes) and converter losses.   |
| **Fuse, connectors, switches, caps, inductor, USB**  |   var   |       –         |           –              |          –                 |        ~0 W           | Passive components; negligible steady-state draw.                                         |

---

## Total Estimated Power Budget

Summing all “Total Power (W)” entries:

\[
0.495 + 0.0825 + 0.0152 + 0.0044 + 0.090 + 0.610 \approx \mathbf{1.30\;W}
\]

At a 5 V input, the converter draws:
\[
I_{\text{in}} = \frac{0.610\ \text{W}}{5\ \text{V}} \approx 0.122\ \text{A}
\]
from the source, for a total input-side power of ~0.61 W.

---

## How the Power Budget Was Used & Conclusions

1. **Estimating Each Load**  
   - We listed every active component (MCU, LEDs, diodes, resistors) and estimated its steady-state current draw based on typical operating conditions.  
   - Passive devices (fuse, connectors, caps, inductor) were omitted from active loads since they draw no continuous power.

2. **Including Converter Efficiency**  
   - The buck converter’s losses were accounted for by dividing the total 3.3 V rail power by its efficiency (≈85 %), ensuring the input-side draw is accurately reflected.

3. **Margin for Peak Currents**  
   - The ESP32 can spike well above its average consumption during Wi-Fi transmissions (~300–400 mA peaks). Based on our 1.3 W average, we recommend the power supply handle at least 1 A at 3.3 V (≈3.3 W) to cover transient peaks.

4. **Supply Selection**  
   - With a 5 V source supplying ~0.12 A average, a standard 5 V/1 A adapter provides sufficient headroom for both average and peak demands, while minimizing heat in the regulator.

5. **Design Implications**  
   - **Thermal:** The regulator must dissipate (5 V–3.3 V) × 0.12 A ≈ 0.204 W. A small PCB copper area or a light heat sink ensures reliable operation.  
   - **Scalability:** Adding more LEDs or sensors will linearly increase load; the budget structure makes it easy to recalculate total needs when expanding the design.

Overall, this budget guided our choice of regulator, adapter rating, and thermal design, ensuring both reliable baseline operation and tolerance for brief current surges.  
