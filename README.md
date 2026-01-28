# Disentangling Latent Risk Pathways via Bayesian Hypergraph Inference (BHPI)

This repository contains the official implementation for: **"Disentangling Latent Risk Pathways via Bayesian Hypergraph Inference."
** BHPI is a scalable Bayesian framework designed to uncover structured latent pathways (hyperedges) from high-dimensional phenotype data.

## 📁 Repository Structure
* `BHPI.m`: Core algorithm implementation using repulsion-aware coordinate ascent variational inference.
* `simulate_design.m`: Main entry point for synthetic experiments and structural recovery benchmarking.
* `helper/`: Directory containing utility functions for synthetic data generation, hypergraph initialization, and repulsion logic.

---

## 🚀 Getting Started

### Prerequisites
* MATLAB (R2023a+)
* Statistics and Machine Learning Toolbox

### Quick Start
To reproduce synthetic experiments, run the following in MATLAB:
```matlab
simulate_design
```


## 📊 Performance Summary

| Phase | Complexity | Speed |
| :--- | :--- | :--- |
| **Training** | $\mathcal{O}(N \cdot E \cdot (P + V))$ | ~117 mins ($N \approx 300k$) |
| **Inference** | Efficient Matrix Ops | $< 0.1$ ms per sample |
