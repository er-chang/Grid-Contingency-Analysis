# Grid-Contingency-Analysis

<img src="https://images.unsplash.com/photo-1769731431903-de98105592e9?q=80&w=1171&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D" width="750">

_Image source: [Scott Greer](https://unsplash.com/photos/fallen-tree-blocks-road-after-ice-storm-7oVpsDc329A)_

## Overview/Objective

Power grids must be designed to withstand unexpected power losses of a single transmission line without violating operating limits. When a transmission line trips, the redistributed power flow can prompt remaining lines to exceed thermal ratings or voltages to drop to unsafe conditions.

The objective is to develop a Python script using the pandapower library to perform N-1 contingency analysis on the IEEE 14-bus test system, identifying transmission line outages that cause overloads.

## Deliverables

| Deliverable                                          |           Location           |
| ---------------------------------------------------- | :--------------------------: |
| Analysis notebook                                    |       `notebook.ipynb`       |
| Network diagrams (base, line0 out, transformer2 out) | `results/net_comparison.png` |
| Hand-calculation check                               |           `docs/`            |
| Daily study log                                      |           `log.md`           |
| Dependencies                                         |      `requirements.txt`      |

## Methodology

- **Initialize the Grid**: Load the standard IEEE-14 bus test case and establish a stable N-0 baseline with thermal ratings set to 150% of the largest base-case line flow (MVA) per voltage class.
- **Vulnerability Screening (N-1)**: Simulate independent component outages (lines, transformers, generators) and mathematically rank grid vulnerabilities using defined voltage (PI_V) and thermal (PI_L) performance indices.
- **Cascading Failure Modeling**: Implement a simulation loop to track successive outages until the grid stabilizes or reaches collapse.
- **Mitigation Strategies**: Evaluate the MW cost and effectiveness of generator redispatch of bus 1 generator and single-load shedding to resolve critical thermal violations.
- **Sensitivity Analysis**: Sweep thermal limit margins (from 1.3x to 2.0x) to test how sensitive the conclusions are to the assumed thermal ratings.

## Results

See [`notebook.ipynb`](./notebook.ipynb) for the complete analysis.

- The intact grid passes all checks (line loading, bus voltage limits, and islanding).
- Of 24 single outage scenarios tested at a 1.5x rating margin, four resulted in violations: lines 0 and 1, and transformers 2 and 3.
- Lines and 1 are the slack bus's only connections to the network. Losing either disconnects the slack bus, isolating up to 259 MW of generation.
- Transformer 2 trips line 10, then 11 (58.2 MW lost). Transformer 3 isolates a bus with no connected load, triggering no subsequent outages.
- A minimum of 18 MW of redispatch prevents line0 cascade with no load lost. Shedding requires at least 21.7 MW.
- No cascades occur above ~1.67x, so the results ultimately depend on the assumed thermal limits.

**Table 1. Effect of redispatching the bus1 generator after line0 outage**

|                     | Before              | After  |
| :------------------ | :------------------ | :----- |
| Redispatch_MW       | 0                   | 18     |
| N1_Status           | Violation           | Pass   |
| Max _Line_Loading_% | 111.1               | 99.7   |
| Worst_Line          | 1                   | 1      |
| Cascade_Status      | Blackout (islanded) | Stable |
| Lines_Tripped       | 2                   | 1      |
| Load_Lost_MW        | 259.0               | 0.0    |

Increasing the bus1 generator by 18 MW reduces the maximum line loading from 111.1% (overloaded) to 99.7% (within thermal limits). The cascade changes from blackout to a stable grid, and no load is lost. However, the system operates close to its thermal limit, requiring additional measures for a more robust solution.

**Table 2: Sensitivity of results to line rating margin.**

|     | Margin | Violations | Elements                                          | Cascades_With_Load_Loss | Worst_Load_Lost_MW |
| --: | -----: | ---------: | :------------------------------------------------ | ----------------------: | -----------------: |
|   0 |    1.3 |          6 | line 0, line 1, line 8, line 11, trafo 2, trafo 3 |                       5 |                259 |
|   1 |    1.5 |          4 | line 0, line 1, trafo 2, trafo 3                  |                       3 |                259 |
|   2 |    1.8 |          1 | trafo 3                                           |                       0 |                  0 |
|   3 |      2 |          1 | trafo 3                                           |                       0 |                  0 |

Higher ratings indicate fewer failures. At 1.3x , six outages fail and five cascades lose load. At 1.5x, four outages fail and three outages lose load. At 1.8x and 2.0x, only transformer 3 is flagged because it islands a bus with no load, and no cascade loses load. Line 0 and line 1 cascades disppear above roughly 1.67%, so these results depend on the assumed ratings.

## References

[1] Pandapower, "Pandapower Documentation." [Online].
Available: https://pandapower.readthedocs.io/en/latest/index.html

[2] University of Washington, "14 Bus Power Flow Test Case." [Online]. Available: https://labs.ece.uw.edu/pstca/pf14/pg_tca14bus.htm

[3] North American Transmission Forum, "NATF TPL-001-4 Reference Document." [Online]. Available: https://www.natf.net/docs/natfnetlibraries/documents/resources/planning-and-modeling/natf-tpl-001-4-reference-document.pdf

[4] Voltage Disturbance, "ANSI C84.1 (Range A)." [Online]. Available: https://voltage-disturbance.com/voltage-quality/voltage-tolerance-standard-ansi-c84-1/

[5] Marquette University, "Predicting Cascading Failures in Power Grids." [Online]. Available: https://epublications.marquette.edu/cgi/viewcontent.cgi?article=1645&context=electric_fac
