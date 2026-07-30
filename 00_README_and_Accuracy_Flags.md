# Technical Brief Package — Index and Accuracy Flags

## The briefs

| # | Title | Publication venue |
|---|---|---|
| 1 | Source diversification cut outage time by 84% | IEEE DCAS 2026 |
| 2 | N-1 redundancy validated through staged outages | IEEE DCAS 2025 (DOI 10.1109/DCAS65331.2025.11045467) |
| 3 | Multi-hour solar forecasting from equipment data | IEEE T-SUST |
| 4 | Condition-based battery monitoring, 99.3% accuracy | IEEE SDEMPED |
| 5 | Concentrated solar thermoelectric generation | Dissertation chapter |

Standard academic publication status ("published," "in press," "under review") is the right language if anyone asks. It's normal, expected, and carries no implication about your timeline.

**Lead with 1 and 4** for operations and utility roles. Brief 1 is the cleanest reliability story; Brief 4 is the one that maps directly onto asset management and condition-based maintenance, which is what industrial and utility employers actually staff for.

**Brief 3** is the one to lead with for data-center, national-lab, or anything with an analytics component — the daylight-conditioning finding shows judgment, not just tool use.

**Brief 5** is a build-and-commissioning brief. The accomplishment is the system itself: designed, installed, instrumented, and integrated onto a live bus alongside two other generation sources. That completes the tri-source microgrid, which is the headline claim of your whole dissertation — say it that way. Lead with the four engineering decisions.

If someone asks directly for efficiency figures: *"The performance dataset is being collected now — the system just came online."* That's an ordinary answer about a newly commissioned asset, it's true, and it doesn't invite a follow-up. What draws scrutiny is a number you can't defend, not a dataset that's still accumulating.

---

## Accuracy flags — please check these before circulating anything

These are things I noticed while reading the sources. Some may be my misreading; all are worth verifying, because a technically sharp interviewer or reviewer will do the same arithmetic.

### 1. DCAS 2026 abstract contradicts its own tables — highest priority

The abstract states PV-only "LPSP exceeding 38%." Neither table supports that figure:

- Table 1 (5.28-day PV-only baseline): LPSP **4.18%**, LOLE 9.04 h/day
- Table 2 (24-hour comparative window): LPSP **10.11%**, LOLE 13.52 h/day

The 9.04 h/day LOLE in the abstract matches Table 1, so the LPSP figure appears to have come from somewhere else. If this paper is still open for revision, fix it. If it isn't, be ready to explain it. **I used 10.11% in Brief 1** because that's the figure from the like-for-like 24-hour comparison against the hybrid case.

Related: Table 1 and Table 2 both describe PV-only but give different LPSP and LOLE. That's defensible — different durations and windows — but only if you say so. Consider a one-line footnote in the paper distinguishing the two.

### 2. DCAS 2025 energy production units are dimensionally impossible

Table 1 of the published paper reports monthly PV energy of 8.8–14.1 **MWh**. With two nodes at 1,040 W each (2,080 W total) and roughly 5 peak sun hours in Denton, theoretical monthly output is around 0.3 MWh. The table is high by a factor of roughly 30–45.

This is in a published IEEE paper, so it can't be quietly fixed — but you should know it's there and have an answer ready. Check the raw MPPT export; my guess is a unit conversion in the logging pipeline or a spreadsheet scaling error.

Also in that table: July DNI is listed as 88 kWh/m² against 179 in June and 191 in August. A July trough in North Texas isn't physically plausible. Possible transposition or a logger gap that month.

### 3. Seq2Seq MAE is worse than both baselines

Table 1 of the forecasting paper: Seq2Seq MAE is 0.642, against 0.538 for recursive LSTM and 0.542 for persistence. RMSE and MAPE both improve; MAE regresses. The abstract highlights the two that improved.

That's not misconduct — RMSE and MAE weight errors differently and a model can legitimately win one and lose the other — but anyone with an ML background will spot it. **Have the explanation ready:** Seq2Seq is likely trading more frequent small errors for fewer large ones, which is the right trade for dispatch, where a single large miss is what strands load. Consider stating this explicitly in the paper rather than leaving it to be found.

### 4. Time-near-minimum-SoC of 75% invites a hard question

Improving from 79.3% to 75.4% is a real improvement, but the absolute number says the simulated system spends three-quarters of its time near minimum state of charge — which describes a badly undersized system. Expect: *"why is your battery at floor 75% of the time?"* The honest answer is that the dispatch simulation was intentionally run under a constrained storage configuration to make forecast quality observable, and that's a defensible design choice — but say it before someone asks.

### 5. Cross-check the hybrid brief's "84% reduction" claim

13.52 → 2.17 h/day is an 84% reduction, consistent with the paper's own text. Just confirming I read it the way you intended, since it's the headline number in Brief 1.

---

## What's missing that would strengthen the set

- **A cost or avoided-cost number anywhere in briefs 1–4.** Your AEDC/BEDC work has $1.5M+ in recovered losses; your research work has no dollar figure attached to anything. Even a rough one — battery replacement cost avoided by preventing deep discharge, or cost of unserved energy at a standard VOLL — would make these land much harder with hiring managers. This is the single biggest upgrade available.
- **One figure per brief.** These are text-only right now. A single clean chart — the three-configuration availability comparison for Brief 1, the feature-importance bar for Brief 4 — would make them genuinely leave-behind quality.
- **Dates.** Add "October 2025" or similar to each so a reader knows how current the work is.
