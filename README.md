# Grid-Contingency-Analysis

<img src="https://images.unsplash.com/photo-1769731431903-de98105592e9?q=80&w=1171&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=NHwxMjA3fDA%3D" width="500">

_Image source: [Scott Greer](https://unsplash.com/photos/fallen-tree-blocks-road-after-ice-storm-7oVpsDc329A)_

## Overview/Objective

Power grids must be designed to withstand unexpected power losses of a single transmission line without violating operating limits. When a transmission line trips, the redistributed power flow can prompt remaining lines to exceed thermal ratings or voltages to drop to unsafe conditions.

The objective is to develop a Python script using the pandapower library to perform N-1 contingency analysis on the IEEE 14-bus test system, identifying transmission line outages that cause overloads.

## Deliverables

TBD

## Methodology

- **Initialize the Grid**: Load the standard IEEE-14 bus test case and establish a stable N-0 baseline with thermal ratings set to 150% of peak MVA per voltage class.
- **Vulnerability Screening (N-1)**: Simulate independent component outages and mathematically rank grid vulnerabilities using defined voltage (PI_V) and thermal (PI_L) performance indicies.
- **Cascading Failure Modeling**: Implement a simulation loop to track successive outages until the grid stabilizes or reaches total collapse.
- **Mitigation Strategies**: Evaluate the MW cost and effectiveness of generator redispatch and load-shedding to resolve critical thermal violations.
- **Sensitivity Analysis**: Sweep thermal limit margins (from 1.3x to 2.0x) to validate the robustness of the cascading failure model.

## Results

TBD

## References

TBD
