
#  Basics of NMOS Drain current (Id) vs Drain-to-source Voltage (Vds)

## 1. Introduction to Circuit Design

### 1.1 Introduction to Basic Element in Circuit Design – NMOS

At the transistor level, all digital circuits are constructed using **complementary MOS (CMOS)** technology — combining **NMOS** and **PMOS** transistors to realize logic functions efficiently.

The **NMOS transistor** forms the foundation of the pull-down network, responsible for discharging the output to ground when the input logic dictates a ‘0’.

* NMOS is a **voltage-controlled device**, where applying voltage at the gate modulates the current between the drain and source.
* Its behavior in a circuit depends on **gate-to-source voltage (V<sub>GS</sub>)**, **drain-to-source voltage (V<sub>DS</sub>)**, and the **threshold voltage (V<sub>T</sub>)**.

In logic design:

* PMOS devices are used in the **pull-up network** (connected to VDD).
* NMOS devices are used in the **pull-down network** (connected to GND).
* Together, they create a **complementary logic stage** that consumes power only during transitions — a key feature of CMOS efficiency.

**Key design point:**
The **width-to-length ratio (W/L)** controls the transistor’s drive strength.

* Higher **W/L** → Higher current drive → Faster switching.
* Lower **W/L** → Lower current drive → Power-efficient but slower response.

In SPICE simulation, this ratio directly affects **VTC (Voltage Transfer Curve)** and **delay behavior**, helping engineers balance speed vs. power.

---

### 1.2 The Critical Role of SPICE Simulation

Even though digital designers rarely run SPICE at the full-chip level, **every gate and standard cell** in a digital library originates from SPICE-level analysis.

SPICE acts as the **golden reference** for transistor-level electrical behavior:

* It models **I–V**, **C–V**, and transient waveforms using physics-based equations.
* Delay and slew metrics for timing analysis are **extracted** directly from these SPICE waveforms.

#### Delay Tables in Cell Characterization

Characterization tools run SPICE simulations across multiple input slews and output loads to populate **lookup tables (LUTs)** — typically stored in `.lib` (Liberty) files used by Static Timing Analysis (STA) tools.

* **Input Slew:** The transition rate of the signal driving the cell.
* **Output Load:** The capacitive load connected to the output pin.
* **Delay:** Time difference between 50% input and 50% output voltage crossings.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/81d96781-4030-4e72-9a5c-f7cf27662cf1" />

SPICE results ensure each cell variant (e.g., BUFX2, BUFX4, INVX1) has calibrated timing data reflecting realistic device physics — essential for **reliable chip signoff**.

---

## 1.3 NMOS Transistor Structure and Threshold Voltage

The **NMOS** (N-channel MOSFET) is fabricated on a **P-type substrate**, with N<sup>+</sup> regions diffused to form **source** and **drain** terminals.
The **gate**, separated from the substrate by a thin oxide, controls the formation of a **conductive channel** under the gate.

The **threshold voltage (V<sub>T</sub>)** is the key defining point where an inversion layer forms, enabling conduction. Below this voltage, only leakage current flows.

| Stage | Gate Voltage                       | Channel Behavior                                               |
| ----- | ---------------------------------- | -------------------------------------------------------------- |
| 1     | V<sub>GS</sub> = 0                 | No conduction; channel absent.                                 |
| 2     | 0 < V<sub>GS</sub> < V<sub>T</sub> | Depletion region forms, but no current flow.                   |
| 3     | V<sub>GS</sub> ≥ V<sub>T</sub>     | Inversion layer connects source to drain; transistor conducts. |

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e339cc39-4a56-4fde-9109-5353b5ddd274" />

The **threshold voltage** depends on:

* **Substrate doping concentration**
* **Oxide thickness (t<sub>ox</sub>)**
* **Body bias (V<sub>SB</sub>)** — leads to the **body effect**, altering V<sub>T</sub> dynamically.

---

### 1.4 Strong Inversion and Threshold Voltage

Strong inversion occurs once V<sub>GS</sub> exceeds V<sub>T</sub> significantly, leading to a dense electron channel at the oxide interface.
At this point, drain current increases almost linearly with V<sub>DS</sub> (for small values) before entering saturation.

**Practical implications:**

* Ensures stable and predictable current flow.
* Critical for analog biasing and digital switching reliability.

---

### 1.5 Threshold Voltage with Positive Substrate Potential

The **body effect** increases threshold voltage when the substrate is biased positively with respect to the source.

The SPICE model incorporates this through:

```
VTH = VTO + γ(√(2φF + VSB) - √(2φF))
```

* **γ (gamma)** = body effect coefficient.
* **VSB** = substrate-to-source bias.
* A higher VSB → increased VTH → reduced current drive.

In digital design, this effect can cause **delay variation** under different substrate bias conditions, hence many circuits keep the body tied to ground (for NMOS) to eliminate this variability.

---

## 2. NMOS Operating Regions

### 2.1 NMOS Resistive Region and Saturation Region of Operation

The NMOS operates in two primary regions depending on V<sub>GS</sub> and V<sub>DS</sub>:

* **Resistive (Linear/Triode) Region:**
  Occurs when V<sub>DS</sub> < (V<sub>GS</sub> − V<sub>T</sub>).
  The device behaves like a **voltage-controlled resistor**.

* **Saturation Region:**
  Occurs when V<sub>DS</sub> ≥ (V<sub>GS</sub> − V<sub>T</sub>).
  The channel is pinched off near the drain, and current becomes nearly constant — ideal for digital switching or current mirrors.


---

### 2.2 Resistive Region of Operation with Small Drain-Source Voltage

At small V<sub>DS</sub>, channel charge is uniform along the channel length.
The drain current is approximately linear:

```
I_DS ≈ μ_n C_ox (W/L) [(V_GS − V_T)V_DS − V_DS²/2]
```

This region is exploited in:

* Analog applications (small-signal amplifiers).
* SPICE fitting for low-voltage operation verification.

---

### 2.3 Drift Current Theory

Electron transport in MOSFETs primarily follows **drift current** behavior:

```
J = q n μ E
```

Where:

* **J:** current density
* **μ:** mobility
* **E:** electric field

Higher electric fields near the drain can reduce mobility (velocity saturation), requiring advanced SPICE models to capture this accurately.

---

### 2.4 Drain Current Model for Linear Region of Operation

Derived from integrating the channel charge across its length, SPICE models the current as:

```
I_DS = μ_n C_ox (W/L) [(V_GS − V_T)V_DS − V_DS²/2]
```

Accurate modeling here ensures precise representation of **ON resistance** and **transition delays** in logic circuits.

---

### 2.5 SPICE Conclusion to Resistive Operation

SPICE uses BSIM (Berkeley Short-channel IGFET Model) equations to model real-world effects:

* Channel length modulation
* Mobility degradation
* Velocity saturation

For accurate simulation, foundry-provided model parameters (from `.lib.spice`) are essential.

---

### 2.6 Pinch-off Region Condition

Pinch-off occurs when:

```
V_DS ≥ V_GS − V_T
```

Beyond this point, increasing V<sub>DS</sub> no longer increases channel current significantly — the transistor enters **saturation**.

In SPICE simulation, this region defines **output characteristics** critical for circuit gain and output swing analysis.

---

### 2.7 Drain Current Model for Saturation Region of Operation

```
I_DS = (1/2) μ_n C_ox (W/L) (V_GS − V_T)² (1 + λV_DS)
```

Here, λ represents **channel length modulation**, introducing a finite output resistance even in saturation — vital for analog and digital accuracy in SPICE models.

---

## 3. Introduction to SPICE Simulation for MOSFETs

### 3.1 Basic SPICE Setup

SPICE requires two primary components:

* **Netlist:** Text description of all devices and their node connections.
* **Model File:** Foundry-provided set of parameters defining transistor physics (e.g., `sky130.lib.spice`).
* 
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/19dc0c16-9bca-4554-aa34-3fbedccf7b41" />

---

### 3.2 MOSFET Models in SPICE

SPICE equations capture all device physics accurately, including body effect, channel-length modulation, and saturation.
It uses advanced models such as **BSIM3** or **BSIM4**, which reflect short-channel effects observed in deep submicron technologies.

---

### 3.3 Inputs to SPICE Engine

**A. Model Parameters:**
Include constants like VTO, γ, KP, λ, and C_ox. These are technology-specific and define transistor response.

**B. Netlist Example:**

```spice
M1 vdd n1 0 0 nmos W=1.8u L=1.2u
R1 in n1 55
Vdd vdd 0 2.5
Vin in 0 2.5
```
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/cd7e17af-7efa-49b4-8f6d-d698c3f1c24a" />

---

### 3.4 Circuit Description in SPICE Syntax

Every element is written in compact line-based syntax. Example:

* **M1** → MOSFET instance.
* **R1** → Resistor.
* **Vdd/Vin** → Voltage sources.

SPICE analyzes node connections and computes voltages and currents over time or bias conditions.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/6419babc-2765-4fab-973c-1f31f8f7ad61" />

---


### 3.5 SPICE Lab with sky130 Models

Setup your environment:

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
```

Run the simulation:

```bash
cd sky130CircuitDesignWorkshop
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```

Analyze **I–V curves** and **waveforms** to validate theoretical models with actual simulation results.



---

## Key Takeaways

* SPICE provides the **gold standard for transistor modeling**.
* NMOS transistor behavior is governed by **V<sub>GS</sub>**, **V<sub>DS</sub>**, and **V<sub>T</sub>**.
* sky130 open-source PDK allows **practical experimentation** with real foundry models.
* Understanding SPICE output enables effective **timing, sizing, and power optimization** in IC design.

