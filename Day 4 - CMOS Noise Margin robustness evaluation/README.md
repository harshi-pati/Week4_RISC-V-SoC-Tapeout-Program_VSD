

# CMOS Noise Margin Robustness Evaluation

This repository focuses on evaluating the **Noise Margin robustness of CMOS inverters** using the **Sky130 PDK** and **Ngspice**.
It explains the principles of noise margins, their dependence on transistor parameters, and demonstrates practical extraction of parameters from the **Voltage Transfer Characteristic (VTC)** curve.

---

## 1. Static Behavior Evaluation – CMOS Inverter Robustness – Noise Margin

### 1.1  Introduction to Noise Margin

Noise Margin defines the robustness of a logic gate (like a CMOS inverter) against disturbances such as crosstalk and glitches—critical in lower-technology nodes.
It quantifies how much noise voltage a gate can tolerate at its input while still producing the correct logical output.

#### Ideal vs Practical Inverter

**Ideal Case**

* Transition around VDD ⁄ 2 is instantaneous.
* Slope (dVout ⁄ dVin) → ∞.

**Practical Case**

* Resistances & capacitances in PMOS/NMOS make the transition gradual.
* Slope ≈ –1 in the transition region.
* Output never reaches exact 0 V or VDD but comes close.

For input 0 – VIL → Output ≈ VOH (logic 1)
For input VIH – VDD → Output ≈ VOL (logic 0)

The mid-region between VIL and VIH is the transition zone.


<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/926f7cba-df7a-469d-8ba2-aac09db38ea4" />

---

### 1.2  Noise Margin Voltage Parameters

#### Basic Inverter Behavior

A CMOS inverter outputs a logic high (VDD) when the input is logic low (0) and vice versa.

The **Voltage Transfer Characteristic (VTC)** curve shows how output (Vout) varies with input (Vin):

* **X-axis:** Input voltage (Vin)
* **Y-axis:** Output voltage (Vout)

At Vin = 0 → Output = VDD
As Vin increases, Vout decreases sharply near Vin ≈ VDD ⁄ 2 (switching threshold).

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/54f63d85-d471-41ca-bd99-03e4809723b9" />

---

### 1.3  Noise Margin Equation and Summary

#### Noise Margins

To ensure reliable logic operation between cascaded stages:

[
NM_H = VOH - VIH
]
[
NM_L = VIL - VOL
]

* **NMH** → Noise tolerance in logic 1 state
* **NML** → Noise tolerance in logic 0 state
* Larger values = better inverter robustness

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/e642928a-4161-41a9-8687-8fcfed8486b0" />

---

### 1.4  Noise Margin Variation with Respect to PMOS Width

* **VOL < VIL** → Ensures the next stage detects logic 0 correctly.
* **VOH > VIH** → Ensures the next stage detects logic 1 correctly.
* Slope near switching point (≈ –1) reflects gain and affects noise immunity.
* Adjusting the **Wp/Wn ratio** shifts the switching threshold—optimizing noise margins and speed.

#### Design Implications

* **Digital Design:** Uses flat VTC regions for noise immunity.
* **Analog Design:** Uses steep VTC region for amplification.
  
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/77896404-fa7a-4ba8-bd7b-3f351e802b4a" />

---

### 1.5  Sky130 Noise Margin Labs

#### Simulation Setup and Execution

To analyze noise margins of the inverter circuit, run:

```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```
<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/4c93be47-d66b-41c7-8c9d-405b9cfa80ec" />

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/0266481e-66f8-4a6e-b531-d8ddc136b557" />

<img width="315" height="91" alt="Image" src="https://github.com/user-attachments/assets/95466ad5-f783-4daa-baf0-c9b2847a5d19" />

---

#### Critical Voltage Points Extraction

**Procedure**

1. Click on **PMOS slope (top)** → Terminal shows `x0 = VIL`, `y0 = VOH`.
2. Click on **NMOS slope (bottom)** → Terminal shows `x1 = VIH`, `y1 = VOL`.

---

## Summary

* **Introduction:** Defined noise margin as a measure of robustness to disturbances.
* **Voltage Parameters:** Explained VTC and switching behavior of CMOS inverters.
* **Equations:** Derived NMH and NML for logic stability assessment.
* **PMOS Width Variation:** Showed how transistor sizing affects noise margin and speed.
* **Sky130 Labs:** Performed Ngspice simulation, plotted VTC, and extracted key values.
