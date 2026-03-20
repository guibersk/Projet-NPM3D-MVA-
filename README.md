# Stochastic Anchor Growing: a Sparse-View Generalization of Scaffold-GS

Guillaume Bernard — Nuages de points et modélisation 3D, MVA 2025-2026

## Repository Structure

This repository contains the code associated with the project report.

- `gaussian_model.py` — Modified version of Scaffold-GS's `gaussian_model.py`,
  implementing the stochastic anchor growing policy (class `GaussianModel`,
  method `anchor_growing`). The three hyperparameters `prob_grow_low_mul` ($\alpha$),
  `prob_grow_obs_bonus` ($\beta$), and `prob_grow_min_obs_ratio` ($\gamma$) are
  defined in `__init__`, and the stochastic growing logic is activated when
  `self.prob_grow = True`.
- `Scaffold_gs_probabilistic.ipynb` — Experiment notebook (see below).

## How to Reproduce the Experiments

Experiments were run on **Google Colab** (GPU: Tesla T4) using **Google Drive**
as persistent storage.

### Google Drive Setup

The following folders are expected in your Google Drive:
```
MyDrive/
├── Scaffold-GS-probgrow/       ← Full Scaffold-GS repo with modified gaussian_model.py
├── datasets/
│   ├── drjohnson_20/           ← DrJohnson dataset, 20% of original views
│   ├── drjohnson_50/           ← DrJohnson dataset, 50% of original views
│   ├── south_building_20/      ← South-Building dataset, 20% of original views
│   └── south_building_50/      ← South-Building dataset, 50% of original views
└── visionary/                  ← ONNX export tool (visionary-laboratory repo)
```

The original Scaffold-GS repository is available at:
https://github.com/city-super/Scaffold-GS

The `gaussian_model.py` provided in this repository replaces the one in
`Scaffold-GS-probgrow/scene/gaussian_model.py`.

### Running the Notebook

Open `Scaffold_gs_probabilistic.ipynb` in Google Colab, mount your Drive,
and run the cells in order. Each training cell launches a `train.py` run with
explicit arguments (scene path, output path, number of iterations, etc.).

Key arguments controlling the stochastic growing policy (set directly in
`gaussian_model.py`):
| Parameter | Variable in code | Role |
|---|---|---|
| $\alpha$ | `prob_grow_low_mul` | Lower gradient threshold multiplier |
| $\beta$ | `prob_grow_obs_bonus` | Under-observation bonus magnitude |
| $\gamma$ | `prob_grow_min_obs_ratio` | Minimum observation gate |

Set `self.prob_grow = False` to revert to the standard Scaffold-GS baseline.

## Visualizing the Results

Reconstructed scenes are exported to `.onnx` format and can be visualized
interactively in the **Visionary viewer**:

👉 https://visionary-laboratory.github.io/visionary/index_visionary.html

Drag and drop any `.onnx` file exported by the notebook into the viewer to
explore the 3D Gaussian reconstruction.
