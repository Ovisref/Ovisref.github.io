# Technical Brief 4
## Condition-Based Battery Monitoring From Existing Controller Data — 99.3% Accuracy, No New Instrumentation

**Ovis Daniel Irefu** | Power Systems | University of North Texas
*Basis: "Machine Learning for Battery Health Monitoring: A CART Approach Using MPPT Charge Controller Data" (manuscript, IEEE SDEMPED track)*

---

### The problem

Standard battery health methods don't survive contact with a live installation. Electrochemical impedance spectroscopy requires taking the bank offline. Coulomb counting accumulates drift. Both need equipment and downtime that operators of distributed storage assets don't have.

### What I built

A Classification and Regression Tree model predicting battery charging state from parameters the charge controller already logs — no added sensors, no disconnection, no proprietary BMS.

- **Dataset:** 429,027 logged records from **two geographically separate sites** — UNT Discovery Park (Denton, TX) and an agricultural shed installation at New Mexico State University — deliberately combined for generalization across climates
- **Target:** MPPT LED state, which the manufacturer maps to defined battery voltage bands and charge stages
- **Features:** battery/array voltage and current, input/output power, RTS, battery and heatsink temperature, charge state (one-hot encoded)
- **Class imbalance:** severe — the rarest class had 237 samples against 134,318 for the largest. Handled with SMOTE on the training split only
- **Tuning:** randomized search with cross-validation; cost-complexity pruning

### What I measured

| Metric | Result |
|---|---|
| Test accuracy | **99.26%** |
| Macro-average F1 | 0.99 |
| Rarest class (Red / undervoltage) recall | 1.00 |
| Dominant feature | Battery voltage — 56% of importance |
| Charge state features | ~42% combined |
| Tree depth after pruning | 25 (from 28), no accuracy loss |

Feature importance landed exactly where battery physics says it should — voltage first, charge phase second, temperature negligible. That's the sanity check that tells you the model learned the system rather than an artifact of the logging.

### Why I chose CART over higher-performing alternatives

Random Forest and SVM were available and are reported in the literature as competitive or better. I used a single decision tree because the output is an explicit if-then rule set an operator or reviewer can read, audit, and disagree with. In an energy management system where a wrong call means a stranded load or a damaged asset, an explainable model that an engineer will actually trust beats a marginally more accurate one they won't.

### Why it matters operationally

1. **Condition-based maintenance from data you already own.** Distributed storage assets are usually monitored by whatever shipped with them. This turns existing telemetry into a health signal.
2. **Catches the cases that matter.** Perfect recall on the undervoltage class — the state that precedes damage — despite it being 0.06% of the dataset.
3. **Deployable at the edge.** Pruned tree, sub-millisecond inference, no cloud dependency.

### Scope of validation

Validated across two sites in different climates, one controller family, one battery chemistry. The label is a charge-state proxy rather than a direct state-of-health measurement — which is the correct scope for a model built on controller telemetry, and the natural extension is broader hardware coverage.

---

**30-second version:** *"Battery health monitoring usually means taking the bank offline or trusting coulomb counting that drifts. I had 429,000 logged records from two sites in different climates, and used them to train a decision tree that predicts charge state from data the controller already produces — 99.3% accuracy, and perfect recall on the undervoltage condition even though it was less than a tenth of a percent of the data. I used a single tree rather than a random forest on purpose: the output is a readable rule set. In an operations context I'd rather have a model the engineer trusts and can audit than one that's a point more accurate and opaque."*
