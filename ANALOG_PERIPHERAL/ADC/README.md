 # 8-Bit SAR-ADC

> **Project:** 8-Bit SAR-ADC — Analog Peripheral for a RISC-V SoC


---
## 1-TOPS proposed architecture   

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/top_architecture.png" title="Figure 1" height="500" width="3000">
<p align="center"> Figure 1: Top level Architecture</p> 

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [what is ADC?](#2-what-is-a-adc)
3. [ADC Architectures](#3-adc-architectures)
4. [SAR-ADC](#4-sar-adc)


----

### **1. Project Overview**

 Analog Signal → ADC → APB (slow bus) → AXI (fast bus) → CPU
<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/ADC_CPU_interface.png" title="Figure 2" height="200" width="600">
<p align="center"> Figure 2: ADC_CPU_interface</p> 

**1.1 ADC (Analog-to-Digital Converter) :**
 - This block takes a real-world analog signal (like voltage from a sensor).
 - It converts it into a digital value (binary number) that the system can understand.
Example: Temperature sensor voltage → digital number like `101101`

**1.2 APB Bus (Advanced Peripheral Bus) :**
 - This is a simple, low-speed communication bus.
 - Used to connect peripherals like ADC, UART, GPIO, etc.
 - The ADC sends its digital data onto this bus.

**1.3 AXI Bus (Advanced eXtensible Interface) :**
 - This is a high-speed, high-performance bus.
 - Used for communication between major system components (like CPU, memory).
 - Data from APB is usually passed to AXI through a bridge (APB-to-AXI bridge).

**1.4 CPU(RISC-V core)**
 - The processor reads the digital data via AXI.
 - It can process, store, or act on the data.

**Design Specification:**

| Parameter | Specification |
|-----------|--------------|
| Resolution | 8 bits (256 levels) |
| Input Range | 0 V to 1.2 V (diffrential)|
| LSB | ~4.7 mV |
| Power | Low (µA-range) |
| Frequency | 1M Hz|

-----

### **2. What is a ADC?**

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/ADC.png" title="Figure 3" height="800" width="3500">
<p align="center"> Figure 3: ADC</p> 

An **Analog-to-Digital Converter (ADC)** is a crucial component that translates continuous, real-world analog signals (like sound, light, temperature) into discrete digital data (0s and 1s) for processing by computers and microcontrollers.

--------

### **3. ADC Architectures**

| Architecture	| Speed	| Resolution	| Typical Use |
|--------------|-------|------------|-------------|
| Flash	| Very high	| Low–Medium	|"RF, high-speed systems"|
| SAR	| Medium	| Medium–High	| Embedded systems |
| Sigma-Delta	| Low	| Very high	| "Audio, sensors" |
| Pipeline	| High	| Medium–High	| "Communication, imaging" |
| Dual-Slope	| Very low	| High	| Multimeters |

--------------------

### **4. SAR-ADC**

A Successive Approximation Register (SAR) ADC is a high-resolution, low-power analog-to-digital converter that uses a binary search algorithm to convert analog signals to digital.

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/SAR-ADC.png" title="Figure 4" height="400" width="800">
<p align="center"> Figure 4: SAR-ADC</p> 

- **Sample & Hold Circuit:** Acquires and holds the input voltage (***Vin***).
- **Comparator:** Compares the input voltage with a trial voltage from the DAC.
- **SAR Logic:** Implements a binary search algorithm to determine each bit of the digital output.
- **Internal DAC (Digital-to-Analog Converter):** Converts the current digital approximation back into a voltage for comparison.


 **SAR-ADC working**

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/SAR_ADC_logic.png" title="Figure 3" height="800" width="800">
<p align="center"> Figure 5: SAR-logic</p> 

I. Start & Sample
 - Start conversion and sample input voltage ***Vin**.
 - Initialize SAR register to 0.

II. Set Trial Bit
  - Set MSB = 1 (trial)
  - Generate corresponding ***Vdac***= ***Vreff/2***

III. Compare
   - Comparator compares ***Vin*** with ***Vdac***

IV. Bit Decision
  - If ***Vin*** > ***Vdac*** → keep bit = 1.
  - Else → reset bit = 0.

V. Repeat Until LSB
 - Move to next lower bit and repeat comparison
 - After all bits are tested → Output final digital code.

       For Example : 3-bit system

       Let Vin = 0.85 v
       - Step-1 : Set MSB = 1 (D = 100) Vdac = Vreff/2 = 0.6v
       - Step-2 : 0.85 > 0.6 → (D = 110) Vdac = mid range of 0.6v to 1.2v = 0.9
       - Step-3 : 0.85 < 0.9 → (D = 101) Vdac = mid range of 0.6v to 0.9v = 0.75
       - Step-4 : 0.85 > 0.75 → (D = 101) : Digital output = 101

----------------

### **5. DAC**

**1. R-2R DAC**
- Uses only two resistor values: R and 2R.
- Each digital bit controls a switch
   - 1 → Connected to Vref
   - 0 → Connected to GND
- Resistor ladder creates binary weighted currentsand the currents combine at output node.
- Works on the concept of voltage divider.
- Output voltage proportional to digital input.

***Vdac = Vref x [D/(2^N)]***
  
**2. Capacitive DAC**
- Uses binary-weighted capacitors (C, 2C, 4C…).
- During sampling → Capacitors store input charge.
- During conversion → Bottom plates switches between:
   - Vref
   - Ground
- Charge redistribution changes output node voltage.

<table>
  <tr>
    <td align="center">
      <img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/CDAC_1.png" height="200" width="350"/>
      <p align="center"> Figure 6: CDAC_Sampling</p> 
    </td>
    <td align="center">
      <img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/CDAC_2.png" height="200" width="350"/>
      <p align="center"> Figure 7: CDAC_Conversion</p> 
    </td>
  </tr>
</table>

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/CDAC_3.png" title="Figure 3" height="800" width="1200">
<p align="center"> Figure 8: CDAC_equivalent</p> 

| Parameter	| R-2R DAC	| CDAC |
|-----------|----------|------|
| Power	 | Static current → Higher power	| No static current → Low power |
| Area	 | Larger for high resolution	| More area-efficient |
| Linearity	| Sensitive to resistor mismatch	| Better matching → Better linearity |
| SAR ADC Suitability	| Needs separate S/H	| Acts as S/H + DAC
