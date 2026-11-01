# Prototype Scope, Data Status and Limitations

NavIce-Antarctica is an academic SIH prototype. Distinguish demonstration capabilities from operational claims:

| Area | Prototype demonstration | Do not infer |
|---|---|---|
| Iceberg tracking | Historical BYU/NIC track processing and visualisation | Continuous real-time tracking of every iceberg |
| Trajectory forecast | Offline GRU and constant-velocity benchmark | Guaranteed future positions or certified accuracy |
| Sea-ice layer | Climatology/advection-style and demonstration visualisation | Validated ConvLSTM forecast from live Sentinel-1 imagery |
| Environmental context | Online request attempts, cache and fallback pathways | Guaranteed live wind/current/ocean observations |
| Collision risk | CPA/TCPA-style calculations and safety envelopes | Certified collision-avoidance advice |
| Route planning | Risk-weighted A* over configured grids and obstacles | A route approved for real vessel operations |
| Digital twin | Animated vessel / iceberg scenario playback | Live ship GPS/AIS telemetry or a physical vessel twin |
| Vessel engineering | Configurable illustrative estimates | Authenticated engine telemetry or class-approved predictions |

## Data provenance
Consult root DATA_SOURCES.md and data/DATASET_MANIFEST.json for provider references. A listed provider is a provenance reference or intended integration source; it does not establish that a live API connection is active in the current run.

## Before operational deployment
- Establish licensed, reliable and monitored data feeds.
- Validate projections, land masks, coordinate systems and temporal alignment.
- Evaluate models on independent regions, seasons and time periods.
- Quantify uncertainty and define conservative fallback behaviour.
- Conduct hazard analysis and maritime domain-expert review.
- Integrate only through certified systems and applicable regulatory processes.
