# ☀️ Dual-Axis Solar Tracker Using Logic Gates as a Controller

A low-cost, microcontroller-free **dual-axis solar tracking system** that uses **Light Dependent Resistors (LDRs)** and **basic digital logic gates** (AND, OR, NOT) to automatically orient a solar panel toward the sun, maximizing energy capture.

> **Course:** EECE 304 — Digital Electronics Laboratory
> **Department:** Electrical, Electronic and Communication Engineering (EECE)
> **Institution:** Military Institute of Science and Technology (MIST)
> **Group:** Group 5

---

## 📑 Table of Contents

- [Abstract](#-abstract)
- [Objectives](#-objectives)
- [Introduction](#-introduction)
- [Background Theory](#-background-theory)
- [System Components](#-system-components)
- [Working Principle](#-working-principle)
- [Block Diagram](#-block-diagram)
- [Circuit Diagram](#-circuit-diagram)
- [Truth Table & Boolean Logic](#-truth-table--boolean-logic)
- [Verilog Implementation](#-verilog-implementation)
- [Simulation Results](#-simulation-results)
- [Advantages & Limitations](#-advantages--limitations)
- [Real-Life Applications](#-real-life-applications)
- [Future Scope](#-future-scope)
- [Conclusion](#-conclusion)
- [Repository Structure](#-repository-structure)
- [References](#-references)
- [Team Members](#-team-members)

---

## 📝 Abstract

This project implements a **dual-axis solar tracking system** that uses four LDR sensors to detect the direction of strongest sunlight and rotates the solar panel along both the **horizontal** and **vertical** axes to stay aligned with the sun. Unlike conventional designs that rely on microcontrollers (e.g., Arduino, PIC), this system performs all decision-making using **basic logic gates**, making it simple, low-cost, and easy to build and troubleshoot.

The LDR readings on the same axis are compared, and the resulting logic decides which way the corresponding DC motor should rotate (clockwise or counter-clockwise) via an **L298N motor driver**. Testing and simulation show that a dual-axis tracker is roughly **10–15% more efficient than a fixed panel** and **8–10% more efficient than a single-axis tracker**.

**Keywords:** solar tracking, single-axis, dual-axis, Light Dependent Resistor (LDR), logic gates, L298N motor driver

---

## 🎯 Objectives

- Design a dual-axis solar tracking system that collects the maximum amount of sunlight on a solar panel.
- Optimize and improve output efficiency compared to fixed and single-axis systems.
- Analyze the performance of different solar tracking approaches to find the most efficient configuration.
- Demonstrate practical, real-world applications of basic digital logic gates and electrical components.

---

## 🌍 Introduction

As global energy demand rises and fossil fuel reserves shrink, renewable sources — particularly solar energy — have become essential to sustainable development. Fixed solar panels lose efficiency as the sun moves across the sky, since they are rarely perpendicular to incoming sunlight. Solar trackers solve this by continuously reorienting the panel toward the sun.

- **Single-axis trackers** follow the sun along one axis (east–west) and improve output by roughly **25–35%** over fixed panels.
- **Dual-axis trackers** follow the sun along *both* axes (horizontal and vertical), increasing output by up to **45%**, making them ideal for regions with significant seasonal variation in sunlight.

This project builds a dual-axis tracker using **basic logic gates** instead of a microcontroller — LDRs sense light intensity, logic gates process the comparisons, and the result drives two DC motors through an L298N driver, achieving real-time tracking without any programming.

---

## 📚 Background Theory

### Solar Power
Solar power converts sunlight into electricity using photovoltaic (PV) cells. When sunlight strikes a PV cell, it excites electrons and generates an electric current — a clean, emission-free energy source suitable for residential, commercial, and industrial use.

### Solar Trackers
A solar tracker automatically reorients a solar panel to stay aligned with the sun:
- **Single-axis trackers** move along one axis (east–west *or* north–south).
- **Dual-axis trackers** move along both axes, tracking the sun's daily *and* seasonal movement for maximum output.

### Logic Gates
Logic gates are the fundamental building blocks of digital circuits, performing operations on binary inputs (`0`/`1`) to produce a binary output:
- **AND gate** — output is `1` only if *all* inputs are `1`.
- **OR gate** — output is `1` if *at least one* input is `1`.
- **NOT (Inverter) gate** — inverts a single input (`1` → `0`, `0` → `1`).

In this project, combinations of these gates process the LDR signals and generate the control signals for the motor driver.

---

## 🔧 System Components

| Component | Description |
|---|---|
| **Solar Panel (PV Module)** | Converts sunlight into DC electricity; typical output ranges from 100–365 W depending on size/conditions. |
| **LDR Sensors (×4)** | Light Dependent Resistors whose resistance decreases as light intensity increases — used to detect the sun's direction. |
| **Logic Gates (AND, OR, NOT)** | Process LDR signals to generate motor control decisions (74HC21, 4073, 4072, 4075 ICs used in simulation). |
| **L298N Motor Driver** | Dual H-bridge IC that controls the speed and direction of the two DC motors. |
| **DC Motors (×2)** | Motor 1 adjusts the **vertical** tilt; Motor 2 adjusts the **horizontal** tilt of the panel. |
| **Breadboard** | Solderless prototyping platform for assembling the circuit. |
| **Jumper Wires** | Used to connect components on the breadboard without soldering. |

![Solar PV System](images/solar-pv-system.png)

---

## ⚙️ Working Principle

The system continuously compares light intensity readings from pairs of LDRs to determine which direction has stronger sunlight, then drives the motors accordingly:

1. **Sensor Placement** — Four LDRs (`S1`–`S4`) are mounted on the panel at different angles, two for each axis.
2. **Vertical Axis Comparison** — The "up-facing" and "down-facing" LDRs are compared. The side with lower resistance (more light) determines whether the panel tilts up or down (**Motor 1**).
3. **Horizontal Axis Comparison** — The "left-facing" and "right-facing" LDRs are compared similarly to control left/right tilt (**Motor 2**).
4. **Logic Gate Processing** — Combinations of AND/OR/NOT gates evaluate the four LDR states (`S1`–`S4`) and generate four control outputs: `CW1`, `CCW1` (Motor 1) and `CW2`, `CCW2` (Motor 2).
5. **Motor Control** — The L298N motor driver receives these logic outputs and drives the two DC motors to physically reposition the panel.
6. **Continuous Feedback** — As the panel moves, the LDR readings change, and the system keeps re-evaluating until the panel is aligned with the sun (no action needed).

This closed-loop process allows the panel to track the sun on both axes using **only sensors and combinational logic — no microcontroller or programming required**.

> 📍 *Sun path data for Dhaka, Bangladesh (used as a reference for tracking range) — generated via [SunCalc](https://www.suncalc.org/).*

![Sun Path at Dhaka](images/sun-path-dhaka.png)

---

## 🧩 Block Diagram

The overall system can be summarized as a simple signal chain: sensing → decision-making → actuation.

![Block Diagram of Dual Axis Solar Tracker](images/block-diagram.jpg)

| Stage | Function |
|---|---|
| **LDR Sensor** | Senses sunlight intensity and outputs a variable resistance/voltage signal. |
| **Combination of Logic Gates** | Compares LDR signals and determines the direction & magnitude of adjustment needed. |
| **L298N Motor Driver** | Converts logic gate outputs into motor drive signals. |
| **Motor 1 / Motor 2** | Physically rotate the panel along the vertical and horizontal axes respectively. |

---

## 🔌 Circuit Diagram

The complete logic-gate circuit was designed and simulated in **Proteus**, combining inverters, AND gates (74HC21, 4073), and OR gates (4072, 4075) to drive an **L298N (L293D in simulation) motor driver** connected to two DC motors.

![Circuit Diagram of Dual Axis Solar Tracker Using Logic Gates as a Controller](images/circuit-diagram.jpg)

---

## 📊 Truth Table & Boolean Logic

The four LDR inputs (`S1`, `S2`, `S3`, `S4`) determine four motor control outputs: `CW1`/`CCW1` for Motor 1, and `CW2`/`CCW2` for Motor 2.

| State | S1 | S2 | S3 | S4 | CW1 | CCW1 | CW2 | CCW2 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 0  | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1  | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |
| 2  | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 |
| 3  | 0 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |
| 4  | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 1 |
| 5  | 0 | 1 | 0 | 1 | — | — | — | — *(No action needed)* |
| 6  | 0 | 1 | 1 | 0 | 0 | 1 | 1 | 0 |
| 7  | 0 | 1 | 1 | 1 | 0 | 0 | 1 | 0 |
| 8  | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 9  | 1 | 0 | 0 | 1 | 0 | 1 | 0 | 1 |
| 10 | 1 | 0 | 1 | 0 | — | — | — | — *(No action needed)* |
| 11 | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |
| 12 | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 1 |
| 13 | 1 | 1 | 0 | 1 | 0 | 0 | 0 | 1 |
| 14 | 1 | 1 | 1 | 0 | 1 | 0 | 0 | 1 |
| 15 | 1 | 1 | 1 | 1 | — | — | — | — *(No action needed)* |

### Simplified Boolean Expressions

Using Boolean algebra (K-map simplification), the truth table reduces to the following minimal SOP expressions, where `S1'` denotes `NOT S1`:

**DC Motor 1 — Clockwise (CW1):**
```
Y(S1,S2,S3,S4) = S1'S2'S4 + S2S3'S4' + S2'S3S4 + S1S2S4'
```

**DC Motor 1 — Counter-Clockwise (CCW1):**
```
Y(S1,S2,S3,S4) = S1'S2S3S4' + S1S2'S3'S4
```

**DC Motor 2 — Clockwise (CW2):**
```
Y(S1,S2,S3,S4) = S1'S2'S4 + S1'S3 + S2'S3S4 + S1S2S3'S4'
```

**DC Motor 2 — Counter-Clockwise (CCW2):**
```
Y(S1,S2,S3,S4) = S2S3'S4' + S1S3' + S1S2S4'
```

This simplification reduces the number of gates and components required, lowering cost and circuit complexity while preserving full functionality.

---

## 💻 Verilog Implementation

The simplified Boolean expressions were translated into Verilog HDL and synthesized/simulated in **Quartus II**.

```verilog
module solar(
    input  wire S1,             // Input signal S1
    input  wire S2,             // Input signal S2
    input  wire S3,             // Input signal S3
    input  wire S4,             // Input signal S4
    output wire DC_Motor1_CW,   // DC Motor 1 - Clockwise
    output wire DC_Motor1_CCW,  // DC Motor 1 - Counter-Clockwise
    output wire DC_Motor2_CW,   // DC Motor 2 - Clockwise
    output wire DC_Motor2_CCW   // DC Motor 2 - Counter-Clockwise
);

    // DC Motor 1 - Clockwise (CW)
    assign DC_Motor1_CW  = (~S1 & ~S2 & S4) | (S2 & ~S3 & ~S4) | (S2 & S3 & S4) | (S1 & S2 & ~S4);

    // DC Motor 1 - Counter-Clockwise (CCW)
    assign DC_Motor1_CCW = (~S1 & ~S2 & S3 & ~S4) | (~S1 & S2 & ~S3 & ~S4);

    // DC Motor 2 - Clockwise (CW)
    assign DC_Motor2_CW  = (~S1 & ~S2 & S4) | (~S1 & S3) | (~S2 & S3 & S4) | (S1 & S2 & ~S3 & ~S4);

    // DC Motor 2 - Counter-Clockwise (CCW)
    assign DC_Motor2_CCW = (S2 & ~S3 & ~S4) | (~S1 & S3) | (S1 & S2 & ~S4);

endmodule
```

| Signal | Meaning |
|---|---|
| `S1`–`S4` | Digital states derived from the four LDR sensors |
| `DC_Motor1_CW` / `DC_Motor1_CCW` | Drive Motor 1 (vertical axis) clockwise / counter-clockwise |
| `DC_Motor2_CW` / `DC_Motor2_CCW` | Drive Motor 2 (horizontal axis) clockwise / counter-clockwise |

---

## 📈 Simulation Results

### Verilog HDL Simulation Waveform
The waveform below confirms that for each combination of `S1`–`S4`, the correct motor control outputs are asserted as defined in the truth table.

![Verilog HDL Simulation Waveform](images/verilog-simulation-waveform.png)

### Output Power Comparison
A comparative study of output power (in watts) over the course of a day shows the dual-axis tracker consistently outperforming both the single-axis tracker and the fixed panel:

![Output Power Comparison: Fixed vs Single-Axis vs Dual-Axis](images/output-power-comparison.png)

**Key findings:**
- Dual-axis tracking improves energy output by roughly **40%** compared to a fixed panel.
- Dual-axis tracking is **10–15% more efficient** than a fixed panel and **8–10% more efficient** than a single-axis tracker overall.

---

## ⚖️ Advantages & Limitations

### ✅ Advantages
- **Cost-effective** — fewer components than microcontroller-based designs, lowering overall system cost.
- **Simple design** — easy to understand, build, and troubleshoot; great for education and small-scale projects.
- **Reliable** — fewer points of failure means reduced maintenance.
- **Fast response** — logic gates react to input changes in real time.
- **Flexible/modular** — gate combinations can be modified to create different control logic.

### ⚠️ Limitations
- **Limited functionality** — logic gates can't implement advanced/adaptive control algorithms.
- **Scalability issues** — more complex tracking logic requires more gates, increasing wiring complexity.
- **No built-in optimization** — lacks learning/adaptive capabilities for variable environmental conditions.
- **Larger circuit footprint** — many discrete gates can make the circuit bulkier than an equivalent microcontroller solution.
- **Noise sensitivity** — basic gates can be more susceptible to signal noise, potentially causing incorrect switching.

---

## 🌐 Real-Life Applications

1. **Increased Energy Output** — Dual-axis trackers can boost panel efficiency by **25–40%** over fixed panels by continuously optimizing orientation.
2. **Performance Across Climates** — Particularly beneficial at high latitudes (large seasonal sun-angle variation) and in regions with intermittent/cloudy sunlight.
3. **Better ROI for Solar Farms** — Maximizes energy output per square meter, improving return on investment for land-constrained installations.
4. **Off-Grid Systems** — Improves reliability for remote applications (telecom towers, water pumps) and reduces dependency on battery storage/backup generators.
5. **Smart Grid & Renewable Targets** — Helps stabilize energy feed-in and supports national renewable energy goals, especially where land is limited.
6. **Cost Considerations** — Higher installation and maintenance costs due to mechanical complexity are offset over time by increased energy yield.
7. **Environmental Impact** — Reduces land footprint per unit of energy generated and lowers reliance on fossil fuels.

---

## 🔮 Future Scope

- Dual-axis tracking is still relatively uncommon commercially, even where solar contributes significantly to the energy mix — single-axis tracking is often considered "good enough." However, dual-axis tracking offers a noticeable efficiency boost.
- **Sensor upgrade** — LDRs are affected by dust and environmental contamination; more robust/precise light sensors could be explored.
- **Cost-benefit analysis at scale** — a full commercial feasibility study of the logic-gate-based approach versus microcontroller-based systems.
- **Structural cost trade-off** — since a reliable tracking structure can be expensive relative to panel cost, adding extra fixed panels instead of a tracking mechanism may sometimes be more cost-effective.

---

## ✅ Conclusion

This dual-axis solar tracking system demonstrates that **basic logic gates alone can effectively replicate the decision-making of a microcontroller-based tracker**, while remaining low-cost, reliable, and easy to maintain. Experimental and simulated results show dual-axis tracking can increase energy output by **~40%** compared to fixed panels, outperforming single-axis systems as well.

By combining LDR sensing, Boolean-simplified logic, and an L298N motor driver, the project provides a practical, programming-free pathway to more efficient solar energy harvesting — a compelling foundation for further refinement toward sustainable, scalable solar tracking solutions.

---

## 📁 Repository Structure

```
.
├── README.md                  # Project overview (this file)
├── images/                     # Diagrams and figures used in this README
│   ├── block-diagram.jpg
│   ├── circuit-diagram.jpg
│   ├── output-power-comparison.png
│   ├── verilog-simulation-waveform.png
│   ├── solar-pv-system.png
│   └── sun-path-dhaka.png
├── docs/
│   ├── Group-5_Project_Report.pdf       # Full project report (PDF)
│   └── Group-5_Presentation.pptx        # Project presentation slides
└── verilog/
    └── solar.v                          # Verilog source for the controller logic
```

> 📌 Add the project report PDF and presentation `.pptx` into the `docs/` folder (create the folder if it doesn't exist), and the Verilog module into `verilog/solar.v` if you'd like the links above to resolve.

---

## 📚 References

- Daily Generation Data Sheet, Bangladesh Power Development Board (BPDB).
- Cheema, Sukhraj Singh. *Simulation studies on dual axis solar photovoltaic panel tracking system.* PhD diss., Thapar University, Patiala, 2012.
- Design and Performance Evaluation of a Dual-Axis Solar Tracking System for Rural Applications — ResearchGate.
- N. Othman, "Performance Analysis of Dual-axis solar tracking system," IEEE Conf., 2013.
- Hossein Mousazadeh et al. (2011), *Journal of Solar Energy Engineering*, Vol. 133.
- [SunCalc.org](https://www.suncalc.org/) — Sun position and path calculator.
- [Karnaugh Map Solver](https://www.charlie-coleman.com/experiments/kmap/) — Boolean logic simplification tool.

---

## 👥 Team Members — Group 5

| SL | Name | Student ID |
|:---:|---|:---:|
| 1 | Md Zain Akhter Bin Helal | 202216030 |
| 2 | Raisul Islam Ratul | 202216049 |
| 3 | Mahmud Hasan Asif | 202216203 |
| 4 | Md Sadman A Rahman | 202216053 |
| 5 | Allama Rejuan | 202116007 |
| 6 | Md. Salman Rashid Himel | 202116169 |
| 7 | Shazia Afrin Anika | 202016083 |

---

*Project for EECE 304 — Digital Electronics Laboratory, Department of EECE, Military Institute of Science and Technology (MIST).*
****
