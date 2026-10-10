# Anomaly Detection in Vehicular Networks

This repository evaluates machine-learning workflows for anomaly detection in simulated vehicular traffic and communication-network measurements. The experiments progress from traffic-event detection to temporal prediction and network KPI monitoring.

## Project workflow

### Phase 1 — Traffic-event detection

SUMO generates traffic traces, including an injected lane-closure event. An Isolation Forest analyzes vehicle-speed features to identify observations that differ from the normal traffic distribution.

### Phase 2 — Predictive traffic monitoring

An LSTM is trained on normal traffic sequences. During a new simulation, the predicted speed is compared with the observed speed; a sufficiently large residual is reported as a potential anomaly.

### Phase 3 — Communication-network monitoring

The repository also includes:

- a live-latency monitor for a network connection;
- synthetic 5G KPI generation using latency and packet-loss features;
- Isolation Forest detection of injected communication anomalies.

## Technologies

- SUMO and TraCI for traffic simulation and control
- Python, Pandas, and NumPy for data processing
- scikit-learn Isolation Forest for unsupervised detection
- TensorFlow/Keras LSTM models for temporal prediction
- Matplotlib, Seaborn, and Plotly for visualization
- `ping3` for latency measurement

## Installation

```bash
git clone https://github.com/BabarHussain786/5G-Vehicular-Anomaly-Detection.git
cd 5G-Vehicular-Anomaly-Detection
pip install pandas numpy scikit-learn tensorflow matplotlib seaborn plotly ping3
```

Install SUMO separately and ensure its tools directory is available through `PATH` or the environment expected by the scripts.

## Running the experiments

Run scripts from the repository root. SUMO experiments require the network and configuration files under the documented `sumo_files/` directory.

### Predictive traffic anomaly detection

Generate routes with SUMO's `randomTrips.py` (adjust the network and simulation horizon to the experiment):

```bash
python /path/to/sumo/tools/randomTrips.py -n sumo_files/map.net.xml -r sumo_files/map.rou.xml -e 86400 -p 4
python scripts/predictive_detector_final.py
python scripts/visualize_results.py
```

The detector writes the measured predictions and anomaly decisions to a CSV file; the visualization script produces the corresponding report image.

### Live latency monitoring

```bash
python scripts/live_latency_detector.py
```

Stop the monitor with `Ctrl+C`. Administrative privileges may be required by the host environment.

## Interpretation

The repository demonstrates an experimental workflow for detecting injected or simulated anomalies. Isolation Forest provides an unsupervised outlier baseline, while the LSTM adds temporal prediction of normal traffic behavior. Reported detections should be interpreted with the simulation configuration, feature definitions, threshold policy, and random seed used to generate them.

## Reproducibility

For each run, record the SUMO and Python versions, simulation configuration, route-generation arguments, random seeds, input-data paths, model hyperparameters, anomaly-injection settings, and output CSV/plot locations. This metadata is required to compare detection results across runs.

## Future extensions

Potential extensions include closed-loop traffic response, multi-feature data fusion, calibrated detection thresholds, and integration with a dedicated network simulator such as NS-3.
