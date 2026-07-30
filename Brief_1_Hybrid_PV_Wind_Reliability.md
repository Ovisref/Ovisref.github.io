# Technical Brief 1
## Source Diversification Cut Outage Time by 84% on an Off-Grid Microgrid

**Ovis Daniel Irefu, P.E. candidate** | Power Systems | University of North Texas
*Basis: IEEE DCAS 2026, "Reliability Assessment of a Hybrid PV–WT Microgrid Systems for Enhanced Resilience"*

---

### The problem

Off-grid and backup power systems built on a single renewable source fail predictably — not from equipment faults, but from the resource itself going away on schedule every night. Most published reliability work on this is simulation-only. Operators need numbers from hardware that actually ran.

### What I built

A reconfigurable 24 V DC-bus microgrid testbed at UNT Discovery Park:

- 1,040 W PV array (8 × YL260C-30b), dual TriStar TS-MPPT-60 charge controllers
- 500 W horizontal-axis wind turbine with hybrid diversion controller (overspeed / overvoltage protection)
- 24 V, 300 Ah battery bank
- Arduino-based PLC as supervisory controller — power flow management, source curtailment, operational logging
- Disconnect and changeover switching allowing PV-only, wind-only, or hybrid operation on the same load

### What I measured

72-hour sequential test, 140 W constant design-basis load, identical conditions across all three configurations:

| Configuration | Availability | Unserved energy (LPSP) | Outage hours/day |
|---|---|---|---|
| PV only | 89.89% | 10.11% | 13.52 |
| Wind only | 17.79% | 82.21% | 12.67 |
| **PV + Wind hybrid** | **99.07%** | **0.93%** | **2.17** |

**84% reduction in outage duration** from adding a generation source that was, on its own, the worst performer in the fleet.

During a 15-hour solar outage (5–6 Sept, near-zero PV output), nocturnal wind generation peaking at 110 W cut cumulative energy deficit by over 76% and kept the battery bank off its depth-of-discharge limit. I formalized this as a **Battery Relief Factor** — a dimensionless index quantifying deficit reduction from hybrid operation relative to a single-source baseline.

### Why it matters operationally

1. **Timing beats capacity.** The wind turbine ran far below rated output and still transformed system availability, because it generated when the load needed it. This is the same logic that governs peaker siting, storage dispatch windows, and N+1 backup design — nameplate is not the deciding variable.
2. **It hits a published standard.** 0.93% LPSP clears the IEEE 2030.9-2019 threshold of 2% for off-grid microgrids; 99.07% availability sits above the 95–99% band typical of rural electrification targets.
3. **It protects the most expensive asset.** Preventing deep-discharge cycles directly extends battery life — the dominant replacement cost in these systems.

### Scope of validation

This is a testbed: ~1 kW of generation, a 140 W constant non-flexible load, and a short test window. The reliability *methodology* and the resource-timing conclusion scale; the absolute numbers do not, and I don't present them as if they do. Component-level fault resilience was not tested — only environmental resource scarcity.

---

**30-second version:** *"I built an off-grid microgrid testbed and ran the same load on solar alone, wind alone, and both together. Solar alone had about 13 and a half hours a day of unserved load. Wind alone was worse — it barely ran. Put them together and outage time dropped to about two hours a day, 99% availability. The lesson was that the wind turbine was valuable not because of how much it produced but because of when — it covered the overnight gap and kept the battery off its discharge floor. That's a resource-timing argument, and it's the same argument you make for storage dispatch or backup sizing anywhere."*
