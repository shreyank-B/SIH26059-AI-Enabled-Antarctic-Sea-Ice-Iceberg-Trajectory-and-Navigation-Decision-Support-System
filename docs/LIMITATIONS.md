# Prototype Scope, Data Status and Limitations

NavIce-Antarctica is an academic SIH prototype. The table separates what the current prototype demonstrates from capabilities that could be added through further data connections, engineering, validation and (where required) certification.

| Area | Current prototype demonstration | What further integration and validation could enable |
|---|---|---|
| Iceberg tracking | Historical BYU/NIC track processing and visualisation | Near-real-time tracking of icebergs covered by available satellite or other observation feeds, with regular ingestion, matching and quality checks. Continuous tracking of every iceberg cannot be guaranteed. |
| Trajectory forecast | Offline GRU and constant-velocity benchmark | Automated forecasts refreshed as new observations arrive; broader regional and seasonal training and independent validation could improve reliability. Future positions and accuracy cannot be guaranteed. |
| Sea-ice layer | Climatology/advection-style and demonstration visualisation | A live or regularly refreshed Sentinel-1 SAR ingestion and preprocessing pipeline could feed a trained ConvLSTM sea-ice forecasting model, after suitable training, validation and uncertainty assessment. |
| Environmental context | Online request attempts, cache and fallback pathways | Reliable wind, ocean-current and wave observations/forecasts from configured provider APIs, subject to provider coverage, licensing, uptime, latency and fallback handling. |
| Collision risk | CPA/TCPA-style calculations and safety envelopes | Risk estimates updated from incoming iceberg, environmental and vessel-position data. Operational collision-avoidance use would still require extensive validation, human oversight and applicable maritime approval; software integration alone does not certify it. |
| Route planning | Risk-weighted A* over configured grids and obstacles | Routes recalculated as new ice, weather, bathymetry and vessel data arrive, with improved constraints and uncertainty-aware costs. Approval for real vessel operations requires independent validation and the relevant operator/regulatory acceptance. |
| Digital twin | Animated vessel / iceberg scenario playback | A live-updating voyage display using authenticated AIS/GPS and iceberg/environmental feeds. A true physical/digital twin would additionally require a validated vessel model and appropriate onboard telemetry; a visual connection alone is not sufficient. |
| Vessel engineering | Configurable illustrative estimates | Estimates informed by authenticated engine and vessel telemetry, if available. Class-approved or safety-critical predictions require validated engineering models, expert review and applicable class/regulatory acceptance. |

## Data provenance

Consult root DATA_SOURCES.md and data/DATASET_MANIFEST.json for provider references. A listed provider is a provenance reference or intended integration source; it does not establish that a live API connection is active in the current run.

## Before operational deployment

- Establish licensed, reliable and monitored data feeds with documented coverage and service-level expectations.
- Validate projections, land masks, coordinate systems, timestamps and temporal alignment.
- Evaluate models on independent regions, seasons and time periods; report uncertainty and failure cases.
- Quantify uncertainty and define conservative fallback behaviour for missing, stale or conflicting inputs.
- Conduct hazard analysis, cybersecurity review and maritime domain-expert assessment.
- Integrate only through appropriately validated systems and applicable regulatory/classification processes.

**Important:** These are technically plausible extension paths, not claims that the integrations are currently implemented or operational. Availability depends on data access, provider terms, engineering effort, validation and any required approvals.
