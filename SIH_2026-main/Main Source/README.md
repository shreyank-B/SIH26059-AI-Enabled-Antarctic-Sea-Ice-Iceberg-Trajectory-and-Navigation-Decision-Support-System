# NavIce-Antarctica
### Smart India Hackathon (SIH) Prototype — Problem Statement 26059

An AI-assisted Antarctic maritime navigation prototype combining the **BYU/NIC Antarctic Iceberg Tracking Database**, **PyTorch GRU** trajectory forecasting, a Gaussian collision-risk engine, and **A\* pathfinding** for route planning. The prototype is served through a **FastAPI + Leaflet interactive geospatial dashboard**.

---

## 🚀 Quick Start

### 1. Create a virtual environment and install dependencies
```bash
python3 -m venv venv
./venv/bin/pip install -r requirements.txt
```

### 2. Run data preprocessing
```bash
PYTHONPATH=. ./venv/bin/python src/preprocessing.py \
  --raw_dir stats_database_v7.1 \
  --output data/processed/iceberg_tracks_clean.csv \
  --top_n 75
```
**Output:** `data/processed/iceberg_tracks_clean.csv` (the pipeline's documented output is 243,433 trajectory points from 75 tracks).

### 3. Train the PyTorch GRU model
```bash
PYTHONPATH=. ./venv/bin/python src/train_gru.py \
  --epochs 10 \
  --batch_size 128
```
**Output:** `models/gru_iceberg.pt` checkpoint and `models/feature_scaler.pkl`. MPS acceleration is used when available; otherwise training runs on the supported CPU path.

### 4. Evaluate models and generate benchmark plots
```bash
PYTHONPATH=. ./venv/bin/python src/evaluate.py
```
**Output:** `models/eval_metrics.json` and `artifacts/trajectory_evaluation.png`.

### 5. Test the route optimizer
```bash
PYTHONPATH=. ./venv/bin/python src/route_optimizer.py --test
```

### 6. Start the interactive dashboard
```bash
PYTHONPATH=. ./venv/bin/uvicorn app.backend.main:app --host 127.0.0.1 --port 8000
```
Open http://127.0.0.1:8000 in your browser.

---

## 📊 Benchmark Results

The figures below are copied from `models/eval_metrics.json` in this repository. The evaluation file reports 46,576 test sequences from the BYU/NIC test tracks.

| Forecast horizon | Model | Mean Haversine error | Median error |
|---|---|---:|---:|
| +24h | Constant-velocity baseline | 2.86 km (1.55 NM) | 0.32 km |
| +24h | **PyTorch GRU** | **2.31 km (1.25 NM)** | **0.20 km** |
| +48h | Constant-velocity baseline | 5.16 km (2.78 NM) | 0.95 km |
| +48h | **PyTorch GRU** | **3.86 km (2.08 NM)** | **0.46 km** |

The evaluation file reports a GRU improvement over the constant-velocity baseline of **19.23% at 24h** and **25.19% at 48h**, measured by mean Haversine error.

---

## 📁 Project Structure

```
data/
  raw/                 # Downloaded BYU/NIC CSV archives
  processed/           # Cleaned tracks and dataset metadata
models/                # GRU checkpoint, scaler, training and evaluation outputs
src/                   # Preprocessing, features, baseline, training, evaluation, routing
app/
  backend/             # FastAPI service
  frontend/            # Leaflet dashboard
artifacts/             # Evaluation plots
requirements.txt
README.md
```

---

## 🌊 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/health` | Server health and model-loading status |
| GET | `/api/stations` | Antarctic research stations and ports |
| GET | `/api/icebergs` | Iceberg catalogue and latest positions |
| GET | `/api/icebergs/{id}/track` | Historical trajectory for an iceberg |
| POST | `/api/predict` | GRU and baseline 24h/48h forecast |
| POST | `/api/route/optimize` | Risk-aware A* route planning |
| POST | `/api/route/replan` | Dynamic route replanning |
| GET | `/api/benchmarks` | Model evaluation metrics |
| GET | `/artifacts/trajectory_evaluation.png` | Benchmark trajectory plot |

---

## 🧠 Model Architecture

**PyTorch GRU sequence model**
- Input: 13-dimensional daily feature vectors (latitude, longitude, velocity, speed, heading encodings, day-of-year encodings, wind and current features).
- Sequence length: 14-day sliding window.
- Architecture: input fully connected layer, two-layer GRU (hidden size 128), pooling head, and multi-horizon regressor.
- Output: displacement predictions for +24h and +48h.
- Training: AdamW, SmoothL1 (Huber) loss, cosine-annealing learning rate, and gradient clipping.

**Constant-velocity baseline**
- Spherical dead-reckoning using the last observed speed and heading.

---

## 🗺️ Route Optimizer

The prototype includes a spatial risk grid, Gaussian iceberg hazard-density estimates, and an 8-connected A* pathfinder. The documented implementation includes Antarctic landmask avoidance, antimeridian wrap-around, and dynamic replanning around a configured iceberg safety perimeter.

---

## 🇮🇳 Indian Antarctic Stations

| Station | Coordinates | Region |
|---|---|---|
| **Maitri** | 70.767°S, 11.733°E | Queen Maud Land |
| **Bharati** | 69.407°S, 76.187°E | Larsemann Hills |

---

## 📦 Dependencies

The Python environment uses PyTorch, NumPy, Pandas, scikit-learn, Matplotlib, FastAPI, Uvicorn, SciPy, and Requests. See `requirements.txt` for the declared package requirements.

---

## ⚠️ Prototype Scope and Limitations

NavIce-Antarctica is a hackathon research prototype, not a certified or operational maritime navigation system. Forecast metrics describe the stored evaluation run and should not be interpreted as a guarantee of future performance. Route and collision-risk outputs are decision-support demonstrations and require validation against current authoritative sea-ice, weather, bathymetry, and vessel data before any real-world use.

The repository includes historical tracking, offline model inference, cached or demonstration environmental context, route-planning logic, and interactive visualizations. It does not claim continuous live AIS/GPS tracking or operationally validated live satellite sea-ice forecasting.

---

## 🔮 Future Enhancements

- Integrate validated, regularly refreshed ERA5 wind and ocean-current fields.
- Explore attention-based sequence models for longer-range dependencies.
- Evaluate hybrid GRU and physics-based drift ensembles.
- Add validated iceberg keel-depth and vessel-draft constraints.
- Integrate authorized AIS vessel feeds with robust freshness and outage handling.
