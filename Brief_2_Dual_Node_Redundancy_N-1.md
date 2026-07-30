# Technical Brief 2
## N-1 Redundancy Validated on Live Hardware Through Staged Outages

**Ovis Daniel Irefu** | Power Systems | University of North Texas
*Basis: IEEE DCAS 2025 (published), "Reliability Analysis of a Dual Photovoltaic (PV) Microgrid: A Case Study on Renewable Power Sharing and Unplanned Outages" — DOI 10.1109/DCAS65331.2025.11045467*

---

### The problem

Redundancy is easy to specify on a single-line diagram and hard to prove. Whether a parallel source actually picks up load without a voltage excursion — and how the system behaves hours and days into a contingency — is a field question, not a design-review question.

### What I built

A dual-node PV microgrid, both nodes tied to a common DC bus for power sharing and redundancy:

- **Per node:** 4 × YL260C-30b 260 W panels (1,040 W), MNPV6 120 A combiner, MPPT charge controller, battery bank (4 × 150 Ah, 12 V)
- Node 1 on a TriStar TS-MPPT-60; Node 2 on a Victron SmartSolar 150/60 — deliberately mixed vendors
- 2,000 W pure sine 24 VDC/120 VAC inverter per node; 60 A/230 V disconnects; lightning arrestor
- Mixed AC and DC load bank: halogen lighting, fan, network router, laptop, Arduino PLC
- MPPTs doubling as data loggers for voltage, current, power, and alarm state

### What I tested

I ran three staged conditions against a seven-month operating baseline:

| Scenario | Condition | Result |
|---|---|---|
| 1 — Normal | Both nodes in service | Baseline diurnal profile, stable bus voltage, minimal end-of-day droop |
| 2 — Short outage | Node-1 PV disconnected, 45 min | Load rode through from 67 W to 1,589 W with **no abrupt voltage or power drop**; clean recovery and recharge on restoration |
| 3 — Prolonged outage | Node-2 PV *and* battery out of service, ~5 days | Surviving node sustained critical loads across five diurnal cycles; overnight load shedding preserved battery capacity |

The step from 67 W to 1,589 W under a failed source is the useful number here — roughly a 24× load increase during a contingency, absorbed without a transient the load could see.

### Why it matters operationally

1. **This is contingency testing, not modeling.** I disconnected real equipment and instrumented what happened. That is the same discipline behind commissioning tests, load-transfer verification, and post-event forensics.
2. **Mixed-vendor coordination worked.** Two different MPPT controllers shared a bus and coordinated charge behavior without custom integration — relevant anywhere heterogeneous equipment has to interoperate on one system.
3. **It found the real weakness.** The prolonged-outage case exposed slow morning ramp under low irradiance — a generation-shape problem, not a capacity problem, and the finding that motivated adding a second resource type (see Brief 1).

### Scope of validation

Testbed scale, with the prolonged-outage case run under moderate irradiance. The transferable contribution is the contingency test methodology and the instrumented evidence behind it — the absolute figures are site-specific by design, and I present them that way.

---

**30-second version:** *"I had a two-node solar microgrid and I wanted to know whether the redundancy on paper was real. So I disconnected a node mid-afternoon while the load was live, then ran a five-day single-node contingency. The short outage rode through a twenty-fold load swing with no visible transient. The five-day test held critical load but exposed a slow morning ramp under cloud — which is what pushed me to add a wind turbine as a complementary resource rather than just oversizing the battery."*
