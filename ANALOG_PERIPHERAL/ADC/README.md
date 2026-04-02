 # 8-Bit SAR-ADC

> **Project:** 8-Bit SAR-ADC — Analog Peripheral for a RISC-V SoC


---
## 1-TOPS proposed architecture   

<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/top_architecture.png" title="Figure 1" height="500" width="3000">
<p align="center"> Figure 1: Top level Architecture</p> 

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [what is ADC?](#2-what-is-a-adc)
3. [ADC Architectures](#3-adc-architectures--comparative-study)


----

### **1. Project Overview**

 Analog Signal → ADC → APB (slow bus) → AXI (fast bus) → CPU
<img src="https://github.com/amitops2103/1-TOPS-SILICON-ANALOG/blob/main/ANALOG_PERIPHERAL/ADC/media/ADC_CPU_interface.png" title="Figure 2" height="200" width="600">
<p align="center"> Figure 2: ADC_CPU_interface</p> 

**1. ADC (Analog-to-Digital Converter) :**
 - This block takes a real-world analog signal (like voltage from a sensor).
 - It converts it into a digital value (binary number) that the system can understand.
Example: Temperature sensor voltage → digital number like `101101`

**2. APB Bus (Advanced Peripheral Bus) :**
 - This is a simple, low-speed communication bus.
 - Used to connect peripherals like ADC, UART, GPIO, etc.
 - The ADC sends its digital data onto this bus.

**3. AXI Bus (Advanced eXtensible Interface) :**
 - This is a high-speed, high-performance bus.
 - Used for communication between major system components (like CPU, memory).
 - Data from APB is usually passed to AXI through a bridge (APB-to-AXI bridge).

**4. CPU(RISC-V core)**
 - The processor reads the digital data via AXI.
 - It can process, store, or act on the data.

**Design Targets:**

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

An Analog-to-Digital Converter (ADC) is a crucial component that translates continuous, real-world analog signals (like sound, light, temperature) into discrete digital data (0s and 1s) for processing by computers and microcontrollers.

--------

## **3. ADC Architectures**
| Architecture	| Speed	| Resolution	| Typical Use |
|--------------|-------|------------|-------------|
| Flash	| Very high	| Low–Medium	|"RF, high-speed systems"|
| SAR	| Medium	| Medium–High	| Embedded systems |
| Sigma-Delta	| Low	| Very high	| "Audio, sensors" |
| Pipeline	| High	| Medium–High	| "Communication, imaging" |
| Dual-Slope	| Very low	| High	| Multimeters |

