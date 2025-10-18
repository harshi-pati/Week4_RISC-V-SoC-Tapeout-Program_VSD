
## **CMOS Power Supply and Device Variation Robustness Evaluation**

### **1. Static Behavior Evaluation – CMOS Inverter Robustness – Power Supply Variation**

#### **1.1 Smart SPICE Simulation for Power Supply Variations**

The robustness of the CMOS inverter with respect to power supply fluctuation is analyzed using SmartSPICE simulations.
The inverter is simulated at different supply voltages such as **1.8 V, 1.6 V, and 2.0 V** to observe the effect on its transfer characteristics.
As the supply voltage decreases, the noise margins shrink, causing a reduction in logic-level differentiation.
The switching threshold (V<sub>m</sub>) also shifts accordingly, showing dependency on V<sub>DD</sub>.
The waveform results show that with lower supply, the rise and fall transitions become slower, confirming degradation in switching performance.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b48afb11-c7b3-42a1-8c75-b10adb36fac4" />

#### **1.2 Advantages and Disadvantages Using Low Supply Voltage**

**Advantages:**

* Reduces **dynamic power consumption** (since P ∝ V<sub>DD</sub><sup>2</sup>).
* Limits **electromigration** and **thermal issues** in interconnects.
* Suitable for low-power, battery-operated circuits.

**Disadvantages:**

* Degraded **noise margins** and **speed** due to reduced drive current.
* **Increased propagation delay** in inverter stages.
* **Reduced robustness** against process and temperature variations.

Hence, while low voltage operation saves power, it introduces significant design trade-offs that must be optimized for stable digital performance.

#### **1.3 Sky130 Supply Variation Labs**

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/5d307794-d901-4507-9334-6b4a69d9aca8" />

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/93eaeeb5-0974-4fcd-ac61-d231efe20689" />

<img width="555" height="85" alt="Image" src="https://github.com/user-attachments/assets/f1c79d9b-53bb-4e8f-aad8-d861cd1f2c93" />

```

Gain = (y0 - y1) / (x0 - x1)

```
---

### **2. Static Behavior Evaluation – CMOS Inverter Robustness – Device Variation**

#### **2.1 Sources of Variation – Etching Process**

Etching variations during fabrication lead to fluctuations in **channel length (L)** and **width (W)** of transistors.
Such deviations modify the transistor current drive and threshold voltage (V<sub>th</sub>), leading to mismatch between NMOS and PMOS devices.
This causes variation in switching threshold and can result in asymmetric VTC characteristics.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/63a2fe26-c3c1-44b7-b079-947353b51bee" />

#### **2.2 Sources of Variation – Oxide Thickness**

Variation in oxide thickness (t<sub>ox</sub>) directly affects the **gate capacitance** and **threshold voltage** of MOS devices.
A thinner oxide results in higher capacitance and faster switching but increases leakage and reduces device reliability.
Conversely, thicker oxide lowers the drive strength, increasing delay and reducing gain.
Thus, maintaining uniform oxide thickness is crucial for consistent inverter performance across wafers.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5433c90d-0c74-41ed-9638-794b386f15aa" />

#### **2.3 Smart SPICE Simulation for Device Variations**

Device variations are simulated in SmartSPICE by adjusting transistor parameters such as **W/L ratio, threshold voltage (Vth), and oxide thickness (t<sub>ox</sub>)**.
The resulting VTC plots show different switching points and varying slopes for each case.
Asymmetry in transition regions and noise margins reflect the mismatch introduced by these variations.
The robustness of CMOS circuits is hence dependent on tight process control to minimize deviation from nominal parameters.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/0e302e64-5899-4a1a-b8dc-8f9850157af3" />

#### **2.4 Conclusion**

From simulation and analysis:

* **Power supply variations** cause direct degradation in inverter performance and noise margins.
* **Device-level variations** from fabrication steps lead to mismatches and instability in operation.
  To design robust CMOS circuits, both these aspects must be considered and mitigated using proper sizing, layout matching, and supply regulation techniques.

#### **2.5 Sky130 Device Variation Labs**

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/789f64ab-af6d-41d2-817f-441f75ee64eb" />

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/0095117f-1271-4825-9a25-d3dcc9376230" />

---

### **Summary**

This experiment demonstrates the impact of **power supply and device variations** on CMOS inverter behavior.
SmartSPICE simulations using the **Sky130 PDK** reveal that lowering supply voltage improves power efficiency but degrades speed and noise margin.
Similarly, device-level process variations cause threshold shifts and mismatches.
Understanding these effects is essential for designing **robust and reliable CMOS logic circuits** in advanced process technologies.

