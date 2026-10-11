A log documenting my understanding of power grid fundamentals, contingency analysis methods, and concepts I had to work through. Each entry typically captures completed tasks, takeaways, and open questions.

---

#### 10/5/2026: Setup and Review

- Updated Python, Git, and VS Code. Created a new repository and cloned it to local workspace. Set up virtual environment and installed pandapower to model power systems. Reviewed three-phase notes from _Electric Energy Conversion_.
- Next: bus types, line and transformer models, power flow

Resource: control.com, "Understanding Electrical Power Grids" ([link](https://control.com/textbook/electric-power-measurement-and-control/electrical-power-grids/))

#### 10/7/2026: Power Grid Fundamentals

- Drafted problem statement and objective.
- What happens when a transmission line exceeds its normal rating? What occurs at each level?
  - normal/continuous rating (100%): safe for indefinite continuous operation
  - long-term emergency (LTE, ~110-115%): safe for 2 to 4 hours following an N-1 contingency, giving operators time to re-dispatch generation
  - short-term emergency (STE, ~120-130%): safe for 15 to 30 minutes to allow immediate automated switching

<details>
<summary>Power Grid Concepts</summary>

**Grid Vocabulary**

- bus (node): where generators, loads, and lines connect
- line (edge): the physical transmission wire connecting buses
- generator: the power plant supplying power to the grid; total generation must always match the sum of total load and line losses
- load: the power being consumed by a city, factory, neighborhood, etc. at a specific bus
- power flow (load flow): mathematical calculation that determines exactly how much power is flowing down each line and what teh voltage is at each bus
- thermal rating (capacity): the maximum physical limit of power a transmission line can withstand before it overheats, sags, and causes a fault
- per-unit (p-u) voltage: a normalized way to measure voltage; grids typically must stay between 0.95 and 1.05 p-u to avoid damaging equipment
- N-1 contingency: the loss of any individual component (a line, generator, or transformer) from a healthy grid

**Transmission Line Ratings**

- SLR (static line rating): the maximum current a transmission line can continuously carry under assumed fixed conditions (worst-case weather conditions are assumed to prioritize safety)
- DLR (dynamic line rating): the maximum current the line can safely carry based on actual, changing conditions

- DLR is an optional upgrade implemented on high-priority lines

**Additional Info**

- Even when a line is tripped, a bus may still receive its required load (consumer demand) by rerouting through another line; however, the other line may pose critical issues that need to be addressed: thermal sag, cascading blackout, etc.
- Meeting consumer demand during N-1 state is not sufficient. If meeting that demand requires any line to exceed 100% thermal loading, the grid is in an insecure state

</details>

#### 10/8/2026: Define Project Deliverables

- Established the normal operating case (N-0) and N-1 cases for the IEEE 14-bus system.
- [AC Power Flow] Non-convergence using the Newton-Raphson method may be an indicator of grid stress, which includes voltage collapse or islanding.
- Are the lines in limits for the benchmark case reasonable? If not, how should they be set?
- Resources:
  - Electrical Engineering Portal, "Learn To Interpret Single Line Diagram" ([link](https://electrical-engineering-portal.com/learn-to-interpret-single-line-diagram))
  - Power Projects, "Contingency Analysis" ([link](https://powerprojectsindia.com/contingency-analysis/))
