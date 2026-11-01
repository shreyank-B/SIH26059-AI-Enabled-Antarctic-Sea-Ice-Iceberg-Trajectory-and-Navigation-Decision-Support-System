# NavIce-Antarctica
### AI-enabled Antarctic sea-ice, iceberg trajectory and maritime navigation decision support

NavIce-Antarctica is a Smart India Hackathon 2026 prototype (Problem Statement **SIH26059**) exploring safer route planning in Antarctic waters. It combines historical iceberg observations, short-horizon trajectory forecasting, collision-risk assessment, risk-aware route search and an interactive voyage simulation.

> **Prototype notice:** This is a research and demonstration decision-support prototype, not a certified navigation system. It does not provide validated operational advice to a real vessel. Simulation playback is not live ship GPS/AIS tracking. Online data requests may fall back to local cached or demonstration data.

## Capabilities
- Historical iceberg tracking and processing using BYU/NIC track data.
- PyTorch GRU trajectory forecasting benchmarked against a constant-velocity baseline.
- CPA/TCPA-style collision-risk calculations and configurable safety envelopes.
- Risk-aware A* route planning with geographic obstacle avoidance.
- Interactive 3D voyage scenario with animated vessel and iceberg hazards.
- Configurable vessel engineering estimates for illustrative resistance, power, fuel and emissions.

## Offline model benchmark
The checked-in models/eval_metrics.json reports mean Haversine errors on the BYU/NIC test-track evaluation:

| Horizon | Constant velocity | GRU | Improvement |
|---|---:|---:|---:|
| 24 hours | 2.86 km | **2.31 km** | 19.23% |
| 48 hours | 5.16 km | **3.86 km** | 25.19% |

These are offline evaluation results, not a guarantee of future real-world accuracy. See docs/MODEL_CARD.md.

## Repository layout
- app/backend — FastAPI service
- app/frontend — browser dashboard and 3D simulation assets
- frontend — Next.js interface source
- src — preprocessing, forecasting, risk and routing modules
- scripts — data synchronisation and stress-test utilities
- data — dataset manifest, cached data and processed outputs
- models — GRU checkpoint, scaler and evaluation artefacts
- artifacts — visualisation and demo artefacts
- docs — architecture, model card and limitations

## Quick start: Python API
Python 3.10+ is recommended.

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
uvicorn app.backend.main:app --host 127.0.0.1 --port 8000
```
Open http://127.0.0.1:8000. API documentation may be available at /docs.

## Frontend
The Next.js source is in frontend/.

```bash
cd frontend
npm ci
npm run dev
```
Configure the frontend API base URL for the backend used in your environment. See frontend/package.json for scripts.

## Reproducing model workflows
Preprocessing, feature generation, training and evaluation modules are in src/. Inspect command-line options and DATA_SOURCES.md before running, as raw dataset paths can differ by environment.

```bash
PYTHONPATH=. python src/preprocessing.py --help
PYTHONPATH=. python src/train_gru.py --help
PYTHONPATH=. python src/evaluate.py --help
PYTHONPATH=. python src/route_optimizer.py --test
```

## Data provenance and status
The manifest references BYU/NIC iceberg tracking, NOAA/NSIDC sea-ice products, Copernicus Sentinel-1, Copernicus Marine/ERA5 and NCPOR resources. A manifest entry identifies a source or intended integration; it does **not** prove that a live feed is currently connected or ingested into every feature.

- Historical BYU/NIC tracks are used for the iceberg trajectory research workflow.
- Some dashboard services attempt online synchronisation and may use local cache or fallback data.
- The sea-ice visual layer currently includes climatology/advection-style or demonstration logic; do not interpret it as a validated operational forecast from a trained ConvLSTM on live Sentinel-1 rasters.
- Environmental forcing and vessel engineering outputs are estimates and depend on configured inputs and available data.
- The 3D voyage is a scenario simulation, not a live AIS feed.

See DATA_SOURCES.md and docs/LIMITATIONS.md.

## Safety and intended use
For academic research, prototyping and demonstration only. Not certified for maritime navigation, icebreaker operations, search and rescue or safety-critical decisions. Operational use would require validated data feeds, independent model verification, uncertainty quantification, domain-expert review, regulatory approval and certified bridge-system integration.

## License
MIT License. Third-party datasets, map layers, models, fonts, images and 3D assets may have separate licences or attribution requirements; consult original providers before redistribution or commercial use.
