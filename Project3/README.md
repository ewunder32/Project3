# Shark Species Classification (CNN) — DS 4002

> **Goal statement:** Develop, train, and evaluate a CNN that classifies shark species from images and integrates conservation status. 

## What’s in this repository
This repo contains code, data pointers, and instructions to reproduce a CNN baseline for shark species classification using a simple TensorFlow/Keras model with three convolutional blocks, plus utilities to link predictions to IUCN conservation status.
---

## 1) Software & Platform

**Language / Framework**
- Python 3.9–3.11
- TensorFlow/Keras (2.12+)
- Google Colab

**Key Python packages (install via `pip`):**
- `tensorflow`
- `numpy`
- `pandas`
- `matplotlib`
- `scikit-learn`
- `Pillow`
- `jupyter`
- `tqdm`
- `seaborn`
- `ipykernel`

**Platforms tested**
- macOS (Apple Silicon/Intel)
- Windows 11
- Ubuntu 22.04 (CPU).



## 2) Map of Documentation
```
Project3
├── DATA
│   ├── Appendix.pdf              # Data dictionary and descriptive statistics
│   ├── README.md                 # How to obtain the raw datasets
│   └── SHARKS_RAYS_CHIMAERAS.dbf  # Raw shark and ray dataset   
├── OUTPUT
│   └── Project 3 Output.pdf      # Final report with results
├── SCRIPTS
│   └── project3.ipynb            # Full dataset creation + analysis pipeline
├── LICENSE.md                    # MIT License for repository code
└── README.md                     # Main Orientation file (this document)

```
