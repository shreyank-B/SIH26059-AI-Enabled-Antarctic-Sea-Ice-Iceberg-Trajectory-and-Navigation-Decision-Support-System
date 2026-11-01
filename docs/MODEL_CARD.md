# Model Card — Iceberg Trajectory GRU

## Purpose
The GRU model is a research prototype for short-horizon forecasting of historical iceberg track movement. It supports model comparison and decision-support experimentation, not independent operational navigation decisions.

## Model and evaluation
- Framework: PyTorch.
- Baseline: constant-velocity / dead-reckoning predictor.
- Forecast horizons: 24 hours and 48 hours.
- Evaluation file: models/eval_metrics.json.
- Evaluation label: BYU/NIC test tracks; 46,576 evaluation sequences.

| Horizon | Predictor | Mean Haversine error |
|---|---|---:|
| 24h | Constant velocity | 2.86 km |
| 24h | GRU | 2.31 km |
| 48h | Constant velocity | 5.16 km |
| 48h | GRU | 3.86 km |

The report records 19.23% lower mean error at 24h and 25.19% at 48h relative to the baseline.

## Intended use
Researchers and student teams evaluating historical iceberg trajectory modelling and route-planning concepts.

## Limitations
Historical test performance does not guarantee performance on new icebergs, unusual ocean conditions or changing climate regimes. Headline metrics alone do not establish calibrated uncertainty. Track observation frequency, data quality, sampling bias and train/test split methodology affect interpretation. Environmental inputs may be cached, approximated or unavailable.

## Responsible use
Do not treat these metrics as a safety guarantee. Operational deployment would require independent validation on temporally and geographically separated data, uncertainty calibration, continuous monitoring, expert review and formal maritime safety assurance.
