
# Velocity Saturation and Basics of CMOS Inverter VTC


## Table of Contents

1. SPICE Simulation for Lower Nodes and Velocity Saturation Effect

* 1.1 SPICE Simulation for Lower Nodes
* 1.2 Drain Current vs Gate Voltage for Long and Short Channel Devices
* 1.3 Velocity Saturation at Lower and Higher Electric Fields
* 1.4 Velocity Saturation Drain Current Model
* 1.5 Labs — Sky130 Id–V<sub>GS</sub> Simulation
* 1.6 Labs — Sky130 Id–V<sub>DS</sub> / V<sub>T</sub> Extraction

2. CMOS Voltage Transfer Characteristics (VTC)

* 2.1 MOSFET as a Switch
* 2.2 Introduction to Standard MOS Voltage–Current Parameters
* 2.3 PMOS/NMOS Drain Current vs Drain Voltage (Load Curves)
* 2.4 Step 1 — Convert PMOS Gate-Source Voltage to V<sub>IN</sub>
* 2.5 Step 2 & 3 — Convert PMOS and NMOS Drain-Source Voltages to V<sub>OUT</sub>
* 2.6 Step 4 — Merge Load Curves and Plot VTC
3. Summary

---

# 1. SPICE Simulation for Lower Nodes and Velocity Saturation Effect

## 1.1 SPICE Simulation for Lower Nodes

**Objective**
Analyze MOSFET I<sub>D</sub> vs V<sub>DS</sub> and I<sub>D</sub> vs V<sub>GS</sub> for different device sizes to highlight short-channel effects and velocity saturation.

**Device cases used (examples)**

* Long-channel device: L = 1.2 µm, W = 1.8 µm
* Short-channel device: L = 0.25 µm, W = 0.375 µm

---

## 1.2 Drain Current vs Gate Voltage for Long and Short Channel Devices

**Long-channel behavior**

* At fixed V<sub>DS</sub> (sufficiently large), I<sub>D</sub> ∝ (V<sub>GS</sub> − V<sub>T</sub>)² in the classical square-law region.
* Quadratic relationship holds over a wide V<sub>GS</sub> range for longer channels — matches long-channel theory.

**Short-channel behavior**

* With reduced L (but same nominal W/L), I<sub>D</sub> deviates from the quadratic law: at higher V<sub>GS</sub> the slope becomes linear rather than quadratic.
* This indicates **velocity saturation** — carriers reach a maximum drift velocity and further V<sub>GS</sub> increases do not produce quadratic increases in current.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/20f18463-de10-42d7-b5a2-d690c03493b9" />

---

## 1.3 Velocity Saturation at Lower and Higher Electric Fields

**Carrier velocity regimes**

* Low-field regime: v = μ·E (mobility-limited — linear).
* High-field regime (beyond critical field E<sub>c</sub>): v → v<sub>sat</sub> (velocity saturation — flattened velocity).

**Impact on drain current**

* For short channels, high lateral electric fields cause carriers to saturate in velocity before the channel reaches the long-channel behavior — hence I<sub>D</sub> transitions from quadratic to linear with V<sub>GS</sub>.
* Velocity saturation reduces effective transconductance and worsens incremental current scaling.

**Design implications**

* Short-channel devices increase switching speed ceilings but complicate analog biasing and predictability.
* SPICE models include velocity saturation parameters (e.g., V<sub>DSAT</sub> or related terms) to capture this behavior.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/111423b4-72c9-4aa0-81c0-3d80b0aada7a" />

---

## 1.4 Velocity Saturation Drain Current Model

**Unified I<sub>D</sub> model (practical form)**

```
I_D = K_n [ V_GT · V_min - (V_min² / 2) ] (1 + λ V_DS)
```

Where:

* V<sub>GT</sub> = V<sub>GS</sub> − V<sub>T</sub>
* V<sub>min</sub> = min(V<sub>GT</sub>, V<sub>DS</sub>, V<sub>DSAT</sub>)
* V<sub>DSAT</sub> = velocity saturation voltage (technology-dependent)
* λ = channel-length modulation factor
* K<sub>n</sub> = μ<sub>n</sub>C<sub>ox</sub>(W/L) (process transconductance parameter)

**How to read it**

* When V<sub>GT</sub> or V<sub>DS</sub> is small, the equation reduces to the long-channel quadratic form.
* When V<sub>DSAT</sub> limits the term (velocity saturation), V<sub>min</sub> locks to V<sub>DSAT</sub> and I<sub>D</sub> grows linearly with V<sub>GS</sub> (not quadratically).
* The (1 + λV<sub>DS</sub>) term models channel-length modulation (non-zero output slope in saturation).

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/91dafbd8-56e6-4110-90e5-3e181f5d5105" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d82c840c-b0dd-454c-8af6-6bbf51dad0a2" />

---

## 1.5 Labs — Sky130 Id–V<sub>GS</sub> Simulation



```bash
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/0e8c7de9-b3f1-4e49-8953-8034cda6bc1a" />
<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/1f1ec829-722c-4724-b92f-f5ff6ca063ee" />

---

## 1.6 Labs — Sky130 Id–V<sub>DS</sub> / V<sub>T</sub> Extraction



```bash
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/f91f1e30-1af2-423a-b520-14ab66612a8b" />

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/3df72c1c-ecff-487f-a6f3-bcedc40515b2" />

---

# 2. CMOS Voltage Transfer Characteristics (VTC)

## 2.1 MOSFET as a Switch

**Core idea**
MOSFET behaves like a voltage-controlled switch:

* NMOS turns ON when V<sub>GS</sub> > V<sub>T</sub>.
* PMOS turns ON when V<sub>GS</sub> < −V<sub>T</sub> (i.e., gate is sufficiently below source/V<sub>DD</sub>).

**Switch modes (in inverter):**

* V<sub>IN</sub> = 0 → PMOS ON, NMOS OFF → V<sub>OUT</sub> pulled to V<sub>DD</sub>.
* V<sub>IN</sub> = V<sub>DD</sub> → NMOS ON, PMOS OFF → V<sub>OUT</sub> pulled to GND.

**Modeling tip**
When analyzing VTC, model each transistor by its I–V (load) curve referenced to V<sub>IN</sub> and V<sub>OUT</sub> only — internal node voltages are implicit.


<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/64680935-b53d-4ee0-a274-6c0794fa86a4" />

---

## 2.2 Introduction to Standard MOS Voltage–Current Parameters

**Common mappings for inverter analysis:**

* NMOS: (V<sub>GSN</sub> = V<sub>IN</sub>), (V<sub>DSN</sub> = V<sub>OUT</sub>)
* PMOS: (V<sub>GSP</sub> = V<sub>IN</sub> − V<sub>DD</sub>), (V<sub>DSP</sub> = V<sub>OUT</sub> − V<sub>DD</sub>)

**Important consequences:**

* PMOS I–V characteristics must be shifted horizontally/vertically when plotted in V<sub>IN</sub>–V<sub>OUT</sub> coordinates.
* Drain current polarities differ: I<sub>DSP</sub> = −I<sub>DSN</sub> when currents are considered with the same sign convention.


<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b6b9da15-5cde-4a40-80ad-2a6b88d623ea" />

---

## 2.3 PMOS/NMOS Drain Current vs Drain Voltage (Load Curves)

**NMOS load curve**

* For fixed V<sub>IN</sub>, plot I<sub>D</sub> vs V<sub>OUT</sub> (V<sub>DSN</sub>). I<sub>D</sub> rises then saturates; saturation point at V<sub>OUT</sub> = V<sub>IN</sub> − V<sub>T</sub>.

**PMOS load curve**

* Mirror image: plotted in shifted coordinates since PMOS source is at V<sub>DD</sub>.

**Use of load curves**

* Intersection of NMOS and PMOS I<sub>D</sub>–V curves at a given V<sub>IN</sub> yields the corresponding steady-state V<sub>OUT</sub>.
* Repeating for V<sub>IN</sub> sweep constructs the VTC.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/42110191-9803-4600-98d9-2986cf4cc43d" />

---

## 2.4 Step 1 — Convert PMOS Gate-Source Voltage to V<sub>IN</sub>

**Transformations**

* V<sub>GSP</sub> = V<sub>IN</sub> − V<sub>DD</sub>
* V<sub>DSP</sub> = V<sub>OUT</sub> − V<sub>DD</sub>

**Interpretation**

* These transform PMOS curves into the V<sub>IN</sub>–V<sub>OUT</sub> frame: effectively shift PMOS characteristics left by V<sub>DD</sub> so they can be overlaid with NMOS load lines.
<img width="1256" height="740" alt="Image" src="https://github.com/user-attachments/assets/1714654e-b562-4f68-ac72-ad2abe43671e" />


---

## 2.5 Step 2 & 3 — Convert PMOS and NMOS Drain-Source Voltages to V<sub>OUT</sub>

**For PMOS**

* V<sub>OUT</sub> = V<sub>DD</sub> + V<sub>DSP</sub>
* With this, V<sub>OUT</sub> = 0 corresponds to V<sub>DSP</sub> = −V<sub>DD</sub> (max charging current);
  V<sub>OUT</sub> = V<sub>DD</sub> corresponds to V<sub>DSP</sub> = 0 (no charging).

**For NMOS**

* V<sub>GSN</sub> = V<sub>IN</sub> and V<sub>DSN</sub> = V<sub>OUT</sub> — direct, no shift required.

**Practical note**

* Plot PMOS curves after shifting so both devices share V<sub>IN</sub> as the independent axis and V<sub>OUT</sub> as the dependent axis.

<img width="1259" height="610" alt="Image" src="https://github.com/user-attachments/assets/2e82a2ea-9cfa-449f-8f37-74d751c32aab" />

---

## 2.6 Step 4 — Merge Load Curves and Plot VTC

**Procedure**

1. For a grid of V<sub>IN</sub> values, compute/plot NMOS I<sub>D</sub> vs V<sub>OUT</sub> and PMOS I<sub>D</sub> vs V<sub>OUT</sub> (after transformation).
2. Find intersections (I<sub>DSN</sub> = I<sub>DSP</sub>) → steady-state V<sub>OUT</sub> for each V<sub>IN</sub>.
3. Plot V<sub>OUT</sub> vs V<sub>IN</sub> to obtain the VTC.

**Typical operating points (example, V<sub>DD</sub> = 2 V):**

* V<sub>IN</sub> = 0 → V<sub>OUT</sub> = V<sub>DD</sub> (PMOS strong ON, NMOS OFF)
* V<sub>IN</sub> ≈ mid-range → both transistors conduct; high gain region around switching threshold (V<sub>M</sub>)
* V<sub>IN</sub> = V<sub>DD</sub> → V<sub>OUT</sub> = 0 (NMOS strong ON, PMOS OFF)

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3d98f010-44ce-4983-9870-a3b517e6173d" />

---

# 3. Summary

* **Velocity Saturation:** In short-channel MOSFETs, carrier velocity stops increasing linearly with the electric field, causing the drain current to shift from a **quadratic** to a **linear** relation with gate voltage.
* **SPICE Observations:** Sky130 simulations confirm early saturation in short-channel devices, showing real-world deviation from ideal models.
* **VTC Behavior:** The CMOS inverter’s VTC defines its switching threshold and noise margins. It results from the intersection of PMOS and NMOS I–V curves.
* **Key Takeaway:** Device scaling improves speed but introduces short-channel and velocity saturation effects that must be modeled carefully.
* **Practical Impact:** Understanding these effects is vital for designing fast, reliable CMOS logic in advanced nodes.
