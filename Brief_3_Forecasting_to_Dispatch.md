# Technical Brief 3
## Multi-Hour Solar Forecasting Built From Equipment Data Already Being Logged

**Ovis Daniel Irefu** | Power Systems | University of North Texas
*Basis: "A Sequence-to-Sequence Deep Learning Model for Solar Power Forecasting in a Microgrid Using Operational Data" (submitted)*

---

### The problem

Most solar forecasting depends on external weather feeds — coarse in space, poorly synchronized in time, and one more vendor dependency. Meanwhile MPPT charge controllers already log array voltage, input power, heatsink temperature, and internal charge state at 5-minute resolution, and nobody is using it. That data is a direct measurement of how *this* system responds to *this* sky.

### What I built

An encoder–decoder (Seq2Seq) LSTM that maps a 60-step lookback window of operational MPPT and weather data to a full 60-step (5-hour) forward power trajectory in a single shot — rather than predicting one step and feeding it back, which compounds error.

- **Features engineered from controller logs:** array voltage, input power, charge state (Bulk / Absorption / Float), battery and heatsink temperature, plus irradiance and temporal features
- **Validation:** leave-one-block-out on time-contiguous operational blocks, preventing leakage across the time series
- **Benchmarks:** persistence baseline and recursive LSTM
- **Downstream:** forecasts embedded in a rule-based dispatch simulator (PV + fixed load + BESS with SoC limits) to measure operational value, not just error

### What I measured

| | Persistence | Recursive LSTM | Seq2Seq LSTM |
|---|---|---|---|
| RMSE | 1.073 | 1.026 | **0.904** |
| MAPE (%) | 275.99 | 248.25 | **110.37** |

More importantly, in dispatch simulation under daylight-conditioned evaluation:

- **Unmet load reduced by ~46 kWh** at short horizons versus the recursive baseline (~10% of simulated unmet load)
- **Time near minimum SoC reduced** from 79.3% to 75.4%
- **Battery throughput up ~13%** without increasing cycling stress
- Error profile stays **flat across the full 5-hour horizon**, where the recursive model degrades with lead time

### The finding I'd actually lead with

The Seq2Seq model initially showed *no* advantage. 45.2% of the dataset was nighttime zeros, and averaging error across a dataset that is nearly half structural zeros masks every real difference between models. Once evaluation was restricted to daylight windows, the operational separation appeared immediately.

That's a data-engineering lesson, not an ML lesson: **an aggregate metric computed over the wrong population will hide the thing you're paying to find.** It generalizes directly to load forecasting, outage statistics, and any KPI averaged across periods where the asset isn't doing anything.

### Why it matters operationally

1. **No new hardware, no new data feed.** The inputs come from equipment already installed and already logging.
2. **Evaluated on dispatch outcomes, not RMSE.** Statistical accuracy that doesn't move unmet load or battery health is not an operational result, and I tested for the difference.
3. **Horizon stability is what dispatch needs.** A forecast that degrades at hour four is unusable for scheduling; flat error across the horizon is the property that matters.

### Scope of validation

Single-site data, rule-based dispatch rather than a real EMS, and a small system. The architecture choice and the daylight-conditioning methodology transfer; the specific kWh figures are site-bound.

---

**30-second version:** *"MPPT controllers log a huge amount of data nobody uses. I built an encoder-decoder LSTM on that operational data instead of a weather feed, and got a five-hour forecast with a flat error profile — recursive models degrade badly at long horizons because they feed their own errors back in. But the real finding was almost a null result: the model showed no benefit at first, because 45% of the dataset was nighttime zeros, which washed out the comparison. Once I conditioned evaluation on daylight windows, unmet load dropped about 46 kWh and time at minimum state of charge improved. The lesson I took was about choosing the evaluation population, not about the architecture."*
