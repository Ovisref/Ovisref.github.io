# Technical Brief 5
## Field-Scale Concentrated Solar Thermoelectric Generator — Built and Commissioned onto a Live 24 V DC Microgrid Bus

**Ovis Daniel Irefu** | Power Systems | University of North Texas
*Solar parabolic dish–thermoelectric generator (PDC-TEG) system, Renewable Power and Environmental Monitoring Research Site, UNT Discovery Park, Denton, TX*

---

### The design problem

Parabolic dish concentrators reach concentration ratios of 800–3,000 and focal temperatures above 500 °C. Thermoelectric generators convert that heat with no moving parts, no working fluid, and effectively no maintenance — but at 2–5% conversion efficiency, and only while a stable temperature gradient is held across the module. The published literature is overwhelmingly simulation-based, and the hardware studies that exist typically omit parasitic cooling load from their efficiency accounting, which flatters the result by a wide margin.

I built the physical system that closes that gap: a third generation source, deployed and commissioned on the same bus as the site's PV and wind assets, completing a tri-source hybrid microgrid.

### System as built

| Subsystem | Implementation |
|---|---|
| Optical | Parabolic dish concentrator, high-reflectivity facets (ρ > 0.9), focal alignment set per z = r²/4f profile |
| Receiver | Cavity receiver with high-conductivity absorber plate, spreading concentrated flux across the TEG mounting surface |
| Conversion | Multi-module Bi₂Te₃ array, series–parallel topology, graphite thermal interface material at all junctions |
| Cold side | Active water-cooling loop, target cold-side temperature below 60 °C |
| Hot-side sensing | MAX31855 SPI amplifiers with K-type thermocouple probes |
| Cold-side sensing | DS18B20 digital sensors |
| Acquisition | ESP32-based logging to InfluxDB, visualization through Node-RED |
| Power conditioning | Boost DC–DC converter to the 24 V DC bus; array configuration developed against both Genasun GVB-8 (5S×4P) and Victron SmartSolar MPPT paths |
| Integration | Tied to the existing 24 V bus alongside 1,040 W PV and a 500 W HAWT, with battery storage and DC protection |

Site conditions: DNI 4.29–5.93 kWh/m²/day during summer months.

### Engineering decisions that define the build

**1. Parasitics sit inside the efficiency boundary.** Cooling pump power counts against net output. This is the single largest reason published PDC-TEG efficiencies overstate real performance, and a figure that excludes parasitics cannot be used to size anything.

**2. Array topology was set by the bus, not by the module.** Open-circuit voltage follows V = NαΔT, so series count is determined by the boost converter's input window and the 24 V bus target — a source-to-bus integration problem before it is a materials problem. The same constraint governs any low-voltage DC source landing on a fixed bus.

**3. The instrumentation resolves gradient *stability*, not peak ΔT.** Most of the literature reports maximum temperature difference achieved. Under real DNI transients, the standard deviation of ΔT is what determines usable output and thermal stress on the ceramic substrates. Sensor selection and sampling architecture were specified around that requirement.

**4. Sensor architecture is split by thermal regime.** Type-K thermocouples with cold-junction-compensated amplifiers on the hot side, where the range demands it; digital one-wire sensors on the cold side, where resolution and channel count matter more than range. Matching sensor technology to the measurement regime rather than standardizing for convenience.

### Measurement framework

Solar-to-thermal efficiency (optical alignment and receiver capture), TEG conversion efficiency (material and ΔT utilization), end-to-end solar-to-grid efficiency (net of parasitics), exergy efficiency, thermal gradient stability σΔT, thermal and electrical time constants, and DC bus voltage deviation under transient irradiance.

### Transferable scope

Concentrator and receiver design, thermal interface engineering, multi-regime sensor architecture, embedded data acquisition, low-voltage DC power conditioning onto a fixed bus, and disciplined efficiency accounting. The same skill set applies directly to waste-heat recovery, process thermal monitoring, and any application where a variable source must be conditioned onto a stable bus.

---

**30-second version:** *"The most recent build is a concentrated solar thermoelectric array — parabolic dish onto a bismuth-telluride module string, water-cooled cold side, boosted onto the same 24-volt bus as the solar and the wind turbine. That completes the tri-source system. The interesting engineering constraint is that the literature reports peak temperature difference, but what actually determines output is how stable that gradient stays under changing irradiance — so I specified the instrumentation to resolve the standard deviation of ΔT, not just the maximum. And I put the cooling pump inside the efficiency boundary, which a lot of published work doesn't, because a number that excludes parasitics can't be used to size a real system."*
