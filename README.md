# ASIC-based-ECG-Arrhythmia-Classifier
This project designs an ASIC-based real-time ECG arrhythmia classifier using approximate computing for low power and high speed. It is a VLSI-domain improvement and continuation of the Nightfall-EX TinyML ECG system, transitioning from embedded implementation to efficient hardware-level cardiac signal processing.


# ASIC-Based Real-Time ECG Arrhythmia Classifier Using Approximate Computing


---

## Project Lineage

This project is a direct hardware upgrade of CardioByte (Nightfall EX)
— our IoT ECG & GPS Tracker built on ESP32.

While CardioByte used a microcontroller with software-based ECG signal
processing, this ASIC project moves the entire ECG classification pipeline
onto dedicated silicon — delivering lower power, faster inference,
and clinical-grade arrhythmia detection that no software solution can match.

| | CardioByte (Nightfall EX) | This Project (ASIC) |
| :--- | :--- | :--- |
| Platform | ESP32 Microcontroller | Custom ASIC Chip |
| ECG Processing | Software (Arduino C) | Hardware RTL (Verilog) |
| Arrhythmia Detection | None | 7-class deep classifier |
| Power | ~200–500 mW | Sub-microwatt target |
| Portability | Wearable prototype | Implantable grade |
| Data Storage | MicroSD card | On-chip memory |
| Communication | GSM SMS + GPS | Standalone ASIC output |

---

## Abstract

Cardiovascular diseases are among the leading causes of death worldwide.
Early detection of heart rhythm abnormalities (arrhythmias) is critical
for preventing severe cardiac events such as heart attacks and strokes.
ECG signals provide vital information about the electrical activity of
the heart and are widely used for arrhythmia detection.

This project designs a low-power ASIC chip that classifies ECG
arrhythmias in real time using approximate computing techniques.
Traditional software-based ECG classifiers consume too much power for
wearable or implantable devices. This system processes ECG signals,
extracts QRS shape features and RR intervals, and classifies heartbeats
into 7 clinically distinct categories using a hardware-efficient deep
neural network — fully implemented at the RTL level in Verilog HDL.

Approximate computing intentionally introduces small, controlled
inaccuracies in arithmetic units to significantly reduce power
consumption, chip area, and processing delay — without affecting
clinical classification accuracy.

---

## Problem Statement

Existing ECG ASIC designs have the following critical flaws:

- Neural network too shallow — identifies only 4 QRS types
- Merges PVC Type II and Paced Beat into one class (serious medical error)
- 80% of chip power wasted on memory storage
- Basic noise filtering — cannot handle real-world motion artifacts
- Requires external AFE and ADC — cannot operate standalone

This project solves all of the above by implementing:
- A deeper neural network correctly classifying all 7 arrhythmia types
- Approximate arithmetic units to cut power and area
- Integrated preprocessing with motion-tolerant filtering
- A standalone ASIC with no external components required

---

## Objectives

- Study ECG signal structure — normal and abnormal heartbeat patterns
- Design ECG signal preprocessing block:
  - Noise and baseline wander removal
  - Motion artifact filtering
- Implement QRS complex detection using Pan-Tompkins algorithm
- Extract features: QRS shape, RR interval, amplitude
- Design a deep neural network classifier in Verilog RTL:
  - Correctly classify all 7 arrhythmia types
  - Fix PVC Type II / Paced Beat merging error in existing designs
- Apply approximate computing to multipliers and adders:
  - Reduce power consumption
  - Reduce chip area
  - Maintain acceptable classification accuracy
- Complete RTL design in Verilog HDL
- Functional simulation using Cadence Xcelium
- Logic synthesis using Cadence Genus
- Physical design — floorplan, placement, CTS, routing (Cadence Innovus)
- Physical verification — DRC and LVS checks (Cadence Virtuoso)
- Generate final GDSII layout file

---

## Arrhythmia Classes

| Class | Full Name | Risk Level |
| :--- | :--- | :--- |
| Normal | Normal Sinus Rhythm | Normal |
| APC | Atrial Premature Contraction | Moderate |
| PVC | Premature Ventricular Contraction | High |
| VT | Ventricular Tachycardia | Critical |
| AF | Atrial Fibrillation | Critical |
| PB | Paced Beat | Monitored |
| EB | Extreme Bradycardia | High |

---

## System Architecture

```text
ECG Signal Input (MIT-BIH Arrhythmia Database)
│
▼
[Stage 1 - Signal Preprocessing]
├── Baseline Wander Removal
├── High-Frequency Noise Filtering
└── Motion Artifact Suppression
│
▼
[Stage 2 - QRS Complex Detection]
├── Bandpass Filtering
├── Differentiation
├── Squaring
├── Moving Window Integration
└── Adaptive Thresholding (Pan-Tompkins)
│
▼
[Stage 3 - Feature Extraction]
├── R-Peak Detection
├── RR Interval Calculation
├── QRS Width and Amplitude
└── Morphology Features
│
▼
[Stage 4 - Deep Neural Network Classifier]
├── Approximate Multipliers (low power)
├── Approximate Adders (low area)
├── Activation Functions (hardware optimized)
└── 7-Class Output Layer
│
▼
[Classification Output]
└── Normal / APC / PVC / VT / AF / PB / EB
```

---

## Approximate Computing

| Unit | Exact Version | Approximate Version | Power Saving |
| :--- | :--- | :--- | :--- |
| Multiplier | 16-bit exact | Approximate MAC | ~40-60% |
| Adder | Ripple carry | Approximate carry | ~30-50% |
| Activation | Full precision | Truncated LUT | ~20-35% |

Small controlled errors in these units do not affect clinical
classification accuracy because biological ECG signals inherently
contain noise and natural variability.

---

## Tools and Software

| Tool | Purpose |
| :--- | :--- |
| Cadence Xcelium | RTL Simulation and Functional Verification |
| Cadence Genus | Logic Synthesis to Gate-level Netlist |
| Cadence Innovus | Physical Design — Floorplan, Place, CTS, Route |
| Cadence Virtuoso | Schematic, Layout, DRC/LVS, GDSII Export |
| MATLAB / Python | ECG Dataset Preprocessing (MIT-BIH Database) |
| Verilog HDL | RTL Design and Implementation |

---

## Implementation Flow

```text
Step 1 ──► Study MIT-BIH Arrhythmia Database
Step 2 ──► ECG preprocessing algorithm design (MATLAB/Python)
Step 3 ──► RTL design in Verilog HDL
Step 4 ──► Functional simulation (Cadence Xcelium)
Step 5 ──► Logic synthesis — gate-level netlist (Cadence Genus)
Step 6 ──► Floorplanning — chip area and power ring definition
Step 7 ──► Standard cell placement (Cadence Innovus)
Step 8 ──► Clock Tree Synthesis (CTS) — clock distribution
Step 9 ──► Routing — metal interconnects
Step 10 ──► Physical verification — DRC and LVS (Cadence Virtuoso)
Step 11 ──► GDSII layout file generation
```
---

## Repository Structure

```text
├── rtl/
│ ├── preprocessing.v
│ ├── qrs_detector.v
│ ├── feature_extractor.v
│ ├── approx_multiplier.v
│ ├── approx_adder.v
│ └── classifier.v
├── sim/
│ └── testbench.v
├── synthesis/
│ └── netlist.v
├── diagrams/
│ ├── architecture_flowchart.png
│ └── block_diagram.png
├── docs/
│ └── synopsis.pdf
└── README.md
```

---

## Base Paper

"A 746 nW ECG Processor ASIC Based on Ternary Neural Network"
Syed Muhammad Abubakar, Yue Yin, Songyao Tan, Hanjun Jiang, Zhihua Wang
IEEE Transactions on Biomedical Circuits and Systems, Vol. 16, No. 4 — August 2022

---

## References

1. Pan and Tompkins — A Real-Time QRS Detection Algorithm, IEEE TBME, 1985
2. Lee et al. — 58-nW ECG ASIC for Wearable Cardiac Monitoring, IEEE TBioCAS, 2015
3. Bhandari et al. — 9.6-uW ECG Processor with Arrhythmia Classification, IEEE TBioCAS, 2020
4. Lee et al. — Low-Power ECG Processor with Adaptive Thresholding, IEEE TBioCAS, 2021
5. Kiranyaz et al. — Real-Time ECG Classification by 1-D CNN, IEEE TBME, 2016
6. Zhang et al. — Heartbeat Classification Using Deep Neural Networks, IEEE Access, 2019
7. Acharya et al. — Deep CNN for Myocardial Infarction Detection via ECG, IEEE Access, 2017
8. Moody and Mark — MIT-BIH Arrhythmia Database, IEEE EMB Magazine, 2001

---

Further details will be updated as the project progresses.
