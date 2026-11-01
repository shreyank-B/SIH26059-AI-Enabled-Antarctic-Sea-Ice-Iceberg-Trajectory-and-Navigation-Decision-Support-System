# NavIce-Antarctica

### AI-enabled Antarctic sea-ice, iceberg trajectory and maritime navigation decision support

NavIce-Antarctica is a Smart India Hackathon 2026 prototype for exploring safer route planning in Antarctic waters. It brings together historical iceberg observations, short-horizon trajectory forecasting, collision-risk assessment, risk-aware route search and an interactive voyage simulation.

> **Prototype notice:** This is an academic research and demonstration prototype, not a certified navigation system. It must not be used to provide operational advice to a real vessel. The voyage simulation is not live ship GPS/AIS tracking. Online data requests may use cached or demonstration data when live services are unavailable.

## Demo video

Watch the NavIce-Antarctica project demonstration: [YouTube demo video](https://youtu.be/MBoCqb07ufw).

## Project at a glance

- **Problem statement:** SIH 2026 — Problem Statement 26059
- **Project:** NavIce-Antarctica
- **Core components:** Python, PyTorch GRU, FastAPI, Next.js, geospatial visualisation and 3D simulation
- **Primary research focus:** Historical iceberg trajectory forecasting and risk-aware Antarctic route-planning concepts

## Key prototype capabilities

- Historical iceberg track processing and visualisation using BYU/NIC data.
- PyTorch GRU trajectory forecasting compared with a constant-velocity baseline.
- CPA/TCPA-style proximity and collision-risk calculations with configurable safety envelopes.
- Risk-aware A* route planning over configured navigation grids and geographic obstacles.
- Interactive dashboard and animated 3D voyage scenario.
- Illustrative vessel engineering estimates based on configured inputs.

Capabilities depend on the data and services available in the current run. See [Prototype limitations](docs/LIMITATIONS.md) before interpreting any output.

## Offline iceberg-model benchmark

The root project model card records the following mean Haversine errors on the BYU/NIC test-track evaluation (46,576 evaluation sequences):

| Forecast horizon | Constant-velocity baseline | GRU | Reported reduction |
|---|---:|---:|---:|
| 24 hours | 2.86 km | **2.31 km** | 19.23% |
| 48 hours | 5.16 km | **3.86 km** | 25.19% |

These are offline evaluation results, not a guarantee of future or operational accuracy. Refer to [docs/MODEL_CARD.md](docs/MODEL_CARD.md) for context and limitations. The older project-level README under `SIH_2026-main/Main Source` may contain earlier benchmark figures; use the root model card and its referenced evaluation artefacts as the current summary, and verify the underlying evaluation before making external claims.

## Repository structure

The repository keeps the original project folder name. The main implementation is under `SIH_2026-main/Main Source/`.

```text
.
├── README.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── LIMITATIONS.md
│   └── MODEL_CARD.md
└── SIH_2026-main/
    └── Main Source/
        ├── app/                 # FastAPI backend and browser dashboard assets
        ├── frontend/            # Next.js frontend source
        ├── src/                 # Data processing, forecasting, risk and routing
        ├── data/                # Dataset manifest, cached and processed data
        ├── models/              # Model checkpoint, scaler and evaluation artefacts
        ├── artifacts/           # Plots and demonstration artefacts
        ├── scripts/             # Utility and workflow scripts
        ├── stats_database_v7.1/
        ├── stats_database_v7.1 2/
        ├── verification_results/
        ├── requirements.txt
        └── DATA_SOURCES.md
```

## Getting started

The commands below assume you have cloned this repository and are running them from its root directory.

### 1. Open the implementation folder

```bash
cd "SIH_2026-main/Main Source"
```

### 2. Set up and run the Python API

Python 3.10 or newer is recommended.

**Windows PowerShell**

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python -m uvicorn app.backend.main:app --host 127.0.0.1 --port 8000
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
python -m uvicorn app.backend.main:app --host 127.0.0.1 --port 8000
```

Then open [http://127.0.0.1:8000](http://127.0.0.1:8000). If the API documentation is enabled by the running application, it may be available at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs).

### 3. Run the Next.js frontend (separate terminal)

From the repository root:

```bash
cd "SIH_2026-main/Main Source/frontend"
npm install
npm run dev
```

Open the local URL printed by Next.js (normally [http://localhost:3000](http://localhost:3000)). Configure the frontend's API base URL for the backend instance you are running; check the frontend source and configuration for the expected setting. The frontend and API are separate processes.

### 4. Explore model and routing workflows

From `SIH_2026-main/Main Source/`, with the Python environment activated:

```bash
python src/preprocessing.py --help
python src/train_gru.py --help
python src/evaluate.py --help
python src/route_optimizer.py --test
```

Check the script help and [DATA_SOURCES.md](SIH_2026-main/Main%20Source/DATA_SOURCES.md) before running data workflows. Some workflows may require additional data downloads, substantial compute or environment-specific paths. Training is not required just to start the API.

## Data provenance and integration status

The project documents references to BYU/NIC iceberg tracks, NOAA/NSIDC sea-ice products, Copernicus Sentinel-1, Copernicus Marine/ERA5 and NCPOR resources. A provider listed in a manifest or data-source document is a provenance reference or intended integration; it does **not** prove that its live feed is connected or currently ingested by every feature.

- Historical BYU/NIC tracks are used in the iceberg trajectory research workflow.
- Some dashboard services attempt online synchronisation and may fall back to local cache or demonstration data.
- The sea-ice visual layer includes climatology/advection-style or demonstration logic. Do not interpret it as a validated operational forecast from a trained ConvLSTM running on live Sentinel-1 rasters.
- Environmental forcing and vessel-engineering outputs are estimates and depend on configured inputs and available data.
- The animated voyage is a scenario simulation, not a live AIS/GPS feed.

See [DATA_SOURCES.md](SIH_2026-main/Main%20Source/DATA_SOURCES.md), [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md), and [docs/LIMITATIONS.md](docs/LIMITATIONS.md).

## Safety and intended use

For academic research, prototyping and demonstration only. Not certified for maritime navigation, icebreaker operations, search and rescue or other safety-critical decisions. Operational use would require licensed and validated data feeds, independent model verification, uncertainty quantification, domain-expert review, regulatory approval and certified bridge-system integration.

## Licence and third-party assets

The repository includes an MIT licence. Third-party datasets, map layers, models, fonts, images and 3D assets may have separate licences or attribution requirements. Check the original provider terms before redistribution or commercial use.
