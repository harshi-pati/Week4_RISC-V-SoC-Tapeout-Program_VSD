
#  **CMOS Switching Threshold and Dynamic Simulations**

## **1. Voltage Transfer Characteristics – SPICE Simulations**

### **1.1 SPICE Deck Creation for CMOS Inverter**

* In this step, the SPICE deck for the CMOS inverter circuit was created using the Sky130 PDK.
* The inverter consists of one PMOS and one NMOS transistor connected in a complementary configuration.
* The PMOS is connected to **VDD**, NMOS to **GND**, and the gates of both transistors are tied together as the **input node**.
* The output is taken from the junction of both transistors.
* Proper **W/L ratios** were assigned for PMOS and NMOS to achieve desired switching behavior.
* This deck serves as the base file for all further simulations.
  
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/319db6fe-2664-48b8-826e-e9397aa04bba" />

---

### **1.2 SPICE Simulation for CMOS Inverter**

* The created SPICE deck was simulated using **Ngspice**.
* The **Voltage Transfer Characteristics (VTC)** of the inverter were obtained by sweeping the input voltage from 0V to VDD.
* The simulation output shows how the output voltage transitions sharply at the switching point.
* This point represents the **threshold voltage (Vm)** of the inverter, where input and output voltages are equal.
* The result verifies correct CMOS operation and symmetrical switching.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/58b46102-0306-4db6-b9fc-83c838992227" />

---

### **1.3 Labs Sky130 SPICE Simulation for CMOS**
  
  **Voltage Transfer Characteristics**

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/6f207f66-f444-4d73-960f-25942ad2e738" />

- Run and plot in Ngspice:
  
```spice
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/bade7a4d-0b63-4a77-b6aa-cbef2b7118b7" />

---

 **Transient Analysis and Delay Calculation**
 
 <img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/e6b579c0-7fde-46e8-886e-1c1cf533eb87" />

- Run and plot in Ngspice:

``` spice
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/a7e76902-cde7-4fa2-8856-021b1ba1caeb" />

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/d3fe0e46-71da-456d-b792-f612a73e1b90" />

---

## **2. Static Behavior Evaluation – CMOS Inverter Robustness (Switching Threshold)**

### **2.1 Switching Threshold, Vm**

* The **switching threshold (Vm)** is the point where the inverter output equals the input voltage.
* Multiple simulations were performed with varying transistor width-to-length ratios ( (W/L)_p ) and ( (W/L)_n ).
* Observations:

  * As ( (W/L)_p ) increases, the inverter’s switching point shifts upward.
  * The threshold voltage ( V_m ) varied approximately from **0.99V to 1.4V**.
* The inverter’s **rise delay** and **fall delay** were also analyzed, showing the dependence of switching speed on transistor sizing.
* **Conclusion:**

  * Larger PMOS width improves logic high robustness but slows down falling transition.
  * Balanced sizing ensures equal rise and fall times for stable operation.
  * Regular inverter or buffer configuration is most suitable for **data path** applications.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5d477e0f-45e0-4227-a1ad-be5b5b98a746" />

---

### **2.2 Analytical Expression of Vm as a Function of (W/L)p and (W/L)n**


### Switching Threshold Voltage Expression

During the switching transition of a CMOS inverter, both the NMOS and PMOS transistors may operate in saturation. The **switching threshold voltage** (<code>V<sub>m</sub></code>) is the input voltage at which the inverter output is at half the supply voltage (<code>V<sub>DD</sub>/2</code>), and it's a critical parameter for noise margin and signal integrity.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d209c3c6-6923-4b6e-9678-b718b0c4b529" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/c7446e70-2da0-4777-b869-8ccb767995e0" />

---

### **2.3 Analytical Expression of (W/L)p and (W/L)n as a Function of Vm**

* This reverse analytical relationship was developed to determine the required transistor sizes for a desired switching threshold.
* The equation allows calculating ( (W/L)_p ) and ( (W/L)_n ) once the target ( V_m ) is specified.
* Such expressions are essential during **inverter design optimization** for speed, area, or power trade-offs.
* This step provides insight into transistor-level tuning for balanced CMOS design.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/289cb2db-5bf3-4652-b196-5372d5d99178" />
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/298af125-31ef-464f-b4b3-833b688e8ae7" />

---

### **2.4 Static and Dynamic Simulation of CMOS Inverter**

* In this simulation, both static and dynamic characteristics were observed for the CMOS inverter.
* Static analysis verified the DC transfer curve and confirmed logic level margins.
* Dynamic simulation involved applying a **pulsed input waveform** to study propagation delays.
* Observations:

  * Delay depends on transistor sizing and capacitive load.
  * Properly balanced inverter ensures minimal rise/fall delay difference.
* This analysis helps determine inverter performance under realistic switching conditions.

---

### **2.5 Extended Static and Dynamic Simulation**

* Further simulation runs were carried out under different capacitive loading conditions.
* The results demonstrated the **impact of load capacitance** on propagation delay and output transition slope.
* These findings confirm the inverse relationship between load and speed — higher load leads to slower switching.
* The results align closely with theoretical delay models and analytical expectations.

---

## **3. Applications of CMOS Inverter in Clock Network and STA**

### **3.1 Applications in Clock Network**

* CMOS inverters are key building blocks of clock distribution networks such as **H-tree** structures.
* They serve as **clock buffers** to drive multiple branches of the clock with equal delay paths.
* Design parameters checked during clock distribution include:

  1. **Clock Skew** – Difference in arrival time of clock edges at various points.
  2. **Pulse Width** – Ensuring consistent high/low time durations.
  3. **Duty Cycle** – Maintaining 50% balance for symmetrical switching.
  4. **Latency** – Time delay between clock source and sink.
  5. **Clock Tree Power** – Optimization for reduced power dissipation.
  6. **Signal Integrity and Crosstalk** – Avoiding interference in closely packed routing.

---

### **3.2 Static Timing Analysis (STA)**

* The inverter’s delay characteristics directly influence **Static Timing Analysis (STA)**.
* STA is performed to ensure timing closure and functional correctness of digital circuits.
* The analysis includes:

  * **Setup Time** and **Hold Time** calculations.
  * Determination of **Data Arrival Time** and **Required Time** for each path.
  * Computation of **Slack = Data Required Time – Data Arrival Time**, which must be ≥ 0 for correct operation.
* Example parameters:

  * Clock period ( T = 1ns )
  * Skew = 10ps
  * Clock uncertainty = 90ps
* Proper inverter design ensures reliable clock signal propagation and minimal skew, which is critical for synchronous designs.
  
<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/c290ebd2-1fd1-4639-85bb-1d6d00546da9" />

---

## **4. Summary**

* The **switching threshold (Vm)** of a CMOS inverter is a critical parameter that depends on transistor sizing ratios.
* Analytical and simulated studies confirm the relationship between ( V_m ) and ( (W/L) ) ratios of PMOS and NMOS.
* Dynamic simulations demonstrate the effect of load capacitance on propagation delay and transition time.
* CMOS inverters form the backbone of **clock networks** and play a crucial role in **Static Timing Analysis (STA)** to ensure synchronization and robustness in digital systems.
* This session deepened understanding of both **static** and **dynamic behavior** of CMOS inverters using **Sky130 technology**.

