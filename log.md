A log documenting my understanding of power grid fundamentals, contingency analysis methods, and concepts I had to work through. Each entry typically captures completed tasks, takeaways, and open questions.

---

#### 10/5/2026: Setup and Review

- Updated Python, Git, and VS Code. Created a new repository and cloned it to local workspace. Set up virtual environment and installed pandapower to model power systems. Reviewed three-phase notes from _Electric Energy Conversion_.
- Next: bus types, line and transformer models, power flow

Resource: control.com, "Understanding Electrical Power Grids" ([link](https://control.com/textbook/electric-power-measurement-and-control/electrical-power-grids/))

#### 10/7/2026: Power Grid Fundamentals

- Drafted problem statement and objective.
- **Q**: What happens when a transmission line exceeds its normal rating? What occurs at each level?
  **A**:
  - normal/continuous rating (100%): safe for indefinite continuous operation
  - long-term emergency (LTE, ~110-115%): safe for 2 to 4 hours following an N-1 contingency, giving operators time to re-dispatch generation
  - short-term emergency (STE, ~120-130%): safe for 15 to 30 minutes to allow immediate automated switching

<details>
<summary>Power Grid Concepts</summary>

**Grid Vocabulary**

- **Bus (Node)**: where generators, loads, and lines connect
- **Line (Edge)**: the physical transmission wire connecting buses
- **Generator**: the power plant supplying power to the grid; total generation must always match the sum of total load and line losses
- **Load**: the power being consumed by a city, factory, neighborhood, etc. at a specific bus
- **Power Flow (Load Flow)**: mathematical calculation that determines exactly how much power is flowing down each line and what teh voltage is at each bus
- **Thermal Rating (Capacity)**: the maximum physical limit of power a transmission line can withstand before it overheats, sags, and causes a fault
- **Per-unit (p-u) voltage**: a normalized way to measure voltage; grids typically must stay between 0.95 and 1.05 p-u to avoid damaging equipment
- **N-1 contingency**: the loss of any individual component (a line, generator, or transformer) from a healthy grid

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

#### 10/12/2026: Performance Index

- Resolved line current calculations to obtain reasonable loading percentages. Ordered N-1 contingencies by performance index.
- PI (performance index) is a score measures how severely the grid is stressed _after_ a component trips; a higher PI indicates a greater impact on the grid.
- **Q**: What adjustments are made to mitigate the effects of single-component failures?
- Next: corrective actions

Resource: EE Power, "Performance Index-Based Security Assessment and Screening Effect: Part 1" ([link](https://eepower.com/technical-articles/performance-index-based-security-assessment-and-screening-effectpart-1/#))

#### 10/13/2026: Resolve Limits

- Fixed false overvoltage flags (prevent 1.09 pu from triggering violations), replaced placeholder line ratings by calculating apparent power, defined a function to check conditions.
- Next: corrective actions

#### 10/15-10/17/2026: Cascading Outages

- Corrected variable references. Created a function to model cascading outages.
- Intentional islanding may be used for cascading-failure mitigation by isolating a vulnerable area before faults cause additional overloads.

<details>
<summary>Corrective Actions</summary>

- **Load Shedding**: the deliberate, temporary shutdown/reduction of electricity supply to selected loads or customers by a utility company or grid operator
- **Generation Redispatch**: adjusting the power output of generators to keep the grid operating safely and reduce grid stress
- **Transformer Tap Changes**: adjusting a transformer's voltage transformation ratio to lower/raise voltage on the connected network
- **Reactive Power Support**: providing or absorbing reactive power through equipment such as capacitor banks and reactors to help maintain acceptable voltage levels
- **Emergency Imports**: obtaining additional electricity from neighboring power grids through interconncections to help relieve local generation shortages
- **Rolling Blackouts**: planned, temporary outages affecting different areas at different times to reduce electricity demand (prevents wider grid collapse)
  - last-resort measure

</details>

- Resources:
  - Marquette University, "Predicting Cascading Failures in Power Grids using Machine
    Learning Algorithms" ([link](https://epublications.marquette.edu/cgi/viewcontent.cgi?article=1645&context=electric_fac))
  - PowerOutage.com, "Power Grid Failure & Rolling Blackout Guide" ([link](https://poweroutage.us/storm-prep/power-grid-failure-rolling-blackout-guide))

#### 10/18/2026: Corrective Actions

- Fixed cascade sim to reveal 2 blackouts. Defined function for N-1 case. Created and implemented redispatch function at bus 1 generator.
- It is vital to create copies of the network for testing while retaining the original model for reference.
- Previous sim_cascade code contained several mistakes: tripped wrong line, unresolved NaN problem, looped forever, altered the original network.

#### 10/19/2026: Corrective Actions (cont.)

- Created table to compare before and after redispatch. Developed a load-shedding function to identify the most effective load to disconnect to minimize cascading failures.
- **Q**: How do grid operators prioritize loads for disconnection during an emergency?
  - **A**: A load shedding hierarchy protects critical infrastructure (e.g., hospitals and public safety networks) by disconnecting lower-priority loads (residential) when necessary.
- Next: cover transformers and generators

Resource: Law Gratis, "Load-Shedding Hierarchy Legal Governance." ([link](https://mail.lawgratis.com/blog-detail/load-shedding-hierarchy-legal-governance))

#### 10/20/2026: Generators & Transformers

- Extended code to include generators and transformers.
- **Q**: What did the addition of generators and transformers reveal about N-1 case?
  - **A**: More failures are brought to light that the line-only study missed. It exposes transformer 2 as being the most overloaded element (~117.38%).
- Next: rating sensitivity sweep
