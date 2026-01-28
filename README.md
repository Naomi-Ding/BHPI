# Disentangling Latent Risk Pathways via Bayesian Hypergraph Inference (BHPI)

This repository contains the official implementation for: **"Disentangling Latent Risk Pathways via Bayesian Hypergraph Inference."** 
BHPI is a scalable Bayesian framework designed to uncover structured latent pathways (hyperedges) from high-dimensional phenotype data.

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


### 🛠 Usage: Model Training and Evaluation
The following example demonstrates how to initialize and call the BHPI algorithm, as implemented in [simulate_design.m](simulate_design.m):
#### 1. Initialization and Core Algorithm Call
Before training, the model requires variational parameter initialization. We recommend using ```NNMF``` for the best performance.
```matlab
% 1. Initialize variational parameters
[initials] = cavi_initialization(seed_init, initial_method, E_hat, X_train, Y_train, []);

% 2. Execute the BHPI Algorithm
model = BHPI(X_train, Y_train, E_hat, max_iter, ...
             seed_init, initials, omega_repulsion, staged, ...
             fix_z, z_constraint, sigma2_alpha, ...
             warmup_iters, batch_size, t0, weights, tol, verbose);
```


#### 2. Prediction and Performance Metric
After training, you can generate predicted probabilities and evaluate the model using the Area Under the Receiver Operating Characteristic (AUROC) curve.
```matlab
% Generate predicted probabilities on a validation/test set
eta_val = X_val * model.beta + model.alpha_mean;
y_fitted_prob_val = 1 ./ (1 + exp(-eta_val));

% Calculate AUROC for each disease (V)
AUROC_val = NaN(1, V);
for v = 1:V
    [~, ~, ~, AUROC_val(v)] = perfcurve(Y_val(:, v), y_fitted_prob_val(:, v), 1);
end
mean_auroc = mean(AUROC_val);
```

#### Key Parameters Explained
- `E_hat`: The maximum number of latent hyperedges to discover.
- `omega_repulsion`: The repulsion strength parameter; setting this $>0$ helps disentangle redundant pathways.
- `initials`: A struct containing the starting values for the variational parameters, can be created by calling `cavi_initialization`.
- `model`: The learned disease-specific risk factor effects.



## 📊 Performance Summary

| Phase | Complexity | Speed |
| :--- | :--- | :--- |
| **Training** | $\mathcal{O}(N \cdot E \cdot (P + V))$ | ~117 mins ($N \approx 300k$) |
| **Inference** | Efficient Matrix Ops | $< 0.1$ ms per sample |
