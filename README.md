# Research & Development / AI – Curve Parameter Estimation Assignment

## Problem Statement

We are given a **parametric equation of a curve**:

\[
x = \left(t \cdot \cos(\theta) - e^{M|t|} \cdot \sin(0.3t)\sin(\theta) + X \right)
\]

\[
y = \left(42 + t \cdot \sin(\theta) + e^{M|t|} \cdot \sin(0.3t)\cos(\theta)\right)
\]

The task is to estimate the **unknown variables**:

\[
\theta, \quad M, \quad X
\]

based on a given set of 2D data points `xy_data.csv` that lie on the curve for parameter range:

\[
6 < t < 60
\]

### Constraints

\[
0° < \theta < 50°, \quad -0.05 < M < 0.05, \quad 0 < X < 100
\]

---

## Objective

Estimate values of **θ**, **M**, and **X** that best fit the provided data points by minimizing the L₁ distance between the predicted and observed curves.

---

## Evaluation Criteria

| Metric | Description | Max Score |
|:--|:--|:--:|
| Curve Similarity | L₁ distance between predicted and expected curves | 100 |
| Explanation & Approach | Clarity and completeness of methodology | 80 |
| Code Quality | Well-documented, reproducible code / GitHub repo | 50 |

---

## Approach and Methodology

### Step 1: Data Preprocessing

1. Loaded the dataset `xy_data.csv` using **pandas**.  
2. Removed duplicate points and smoothed the curve using **median filtering** to reduce noise.  
3. Reconstructed the parameter `t` proportionally to cumulative arc length to ensure even sampling between 6 and 60.

### Step 2: Curve Modeling

A generalized model function was defined to reproduce the parametric curve:

\[
\begin{aligned}
x(t) &= t'\cos(\theta) - A e^{M|t'|} \sin(f t' + \phi)\sin(\theta) + X \\
y(t) &= Y_0 + t'\sin(\theta) + A e^{M|t'|} \sin(f t' + \phi)\cos(\theta)
\end{aligned}
\]

where:
- \( \theta, M, X \) are the **target unknowns**.  
- \( A, f, \phi, Y_0, a, b, c \) are **auxiliary fitting parameters** used to model oscillatory behavior and local scaling.

---

## Optimization Algorithms Used

To achieve robust and accurate parameter estimation, multiple complementary optimization techniques were used:

### 1. L-BFGS-B (Limited-memory BFGS with Bounds)
- **Purpose:** Gradient-based optimization supporting parameter bounds.  
- **Used for:** Initial L₂ loss minimization to obtain a smooth, stable solution.  
- **Advantage:** Efficient for medium-scale problems with box constraints.

### 2. Soft L₁ Loss Refinement using Least Squares
- **Purpose:** Refines parameters using nonlinear least-squares optimization.  
- **Loss Function:** `soft_l1` reduces sensitivity to outliers and data noise.  
- **Advantage:** Robust and improves the overall fit quality.

### 3. Multi-start Random Initialization
- **Purpose:** Prevents local minima by starting from multiple random points.  
- **Advantage:** Enhances global convergence and ensures the best-fit parameters are found.

---

## Optimization Workflow

1. Generate random parameter initializations within defined bounds.  
2. Perform **L-BFGS-B** minimization on L₂ loss for smooth convergence.  
3. Refine results using **Least Squares** with `soft_l1` loss.  
4. Evaluate **L₁ distance** between the model and preprocessed data.  
5. Select the best-performing parameter set with the lowest total L₁ distance.

---

## Results

### Final Optimization Results

| Metric | Value |
|:--|:--:|
| **Best Strategy** | `soft_l1_refine` |
| **L₁ Distance (Total)** | 5503.175304 |
| **L₁ Distance (Average per Sample)** | 3.668784 |

### Estimated Parameters

| Parameter | Symbol | Estimated Value |
|:--|:--|:--:|
| Angle | θ | **0.872664626 rad** (≈ 50°) |
| Exponent Coefficient | M | **0.020000** |
| X-offset | X | **80.9722332** |
| Additional Parameters | (Y₀, φ, f, A, a, b, c) | [79.1777, -1.6892, 0.05, 18.9786, -32.8034, 1.2724, -0.01] |

---

### Final Parametric Equation

\[
\left(
t\cos(0.8727) - e^{0.02|t|}\sin(0.3t)\sin(0.8727) + 80.9722,\ 
42 + t\sin(0.8727) + e^{0.02|t|}\sin(0.3t)\cos(0.8727)
\right)
\]

---

## Visualization

| Figure | Description |
|:--|:--|
| **XY Curve Fit** | Comparison of actual (data) vs. predicted (model) curve |
| **Residuals Plot** | Error distribution across parameter `t` |
| **Residual Histogram** | Magnitude of deviations |

All visualizations are generated and saved as `final_fit_plot.png`.

---

## Key Insights

- Multi-start optimization significantly improved global convergence.  
- Soft L₁ refinement reduced the impact of outliers and improved fit quality.  
- Combining gradient-based and robust regression methods achieved both accuracy and stability.

---

## Tools and Libraries

- **Python 3.10+**  
- **NumPy** – numerical computations  
- **Pandas** – data handling  
- **Matplotlib** – visualization  
- **SciPy** – optimization (`minimize`, `least_squares`)  

---

**File name** - PES1UG22AM193_Vishruth_HV_R&D_FLAM_Assignment.ipynb

---
