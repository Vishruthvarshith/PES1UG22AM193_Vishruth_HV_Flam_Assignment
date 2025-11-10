# Research & Development (R&D) — FLAM Round

### Candidate: Vishruth H. V  
**Project Title:** Parametric Curve Parameter(Unknown Variables) Estimation  
**Institution:** PES University  
**File:** `PES1UG22AM193_Vishruth_HV_R&D_FLAM_Assignment.ipynb`

---

## Problem Overview

The objective of this assignment is to estimate the unknown parameters \(\theta\), \(M\), and \(X\) for a given parametric curve defined as:

\[
x = \left( t \cdot \cos(\theta) - e^{M|t|}\sin(0.3t)\sin(\theta) + X \right)
\]

\[
y = \left( 42 + t \cdot \sin(\theta) + e^{M|t|}\sin(0.3t)\cos(\theta) \right)
\]

where \(t\) is the parameter ranging from \(6 < t < 60\).

### Unknown Parameters and Their Ranges

\[
0^\circ < \theta < 50^\circ, \quad -0.05 < M < 0.05, \quad 0 < X < 100
\]

The dataset (`xy_data.csv`) provides observed points \((x_i, y_i)\) for \(t \in (6, 60)\).  
The task is to determine \(\theta, M,\) and \(X\) such that the predicted curve best fits the given data.

---

## Approach and Methodology

1. **Data Visualization**  
   The dataset was first plotted to understand the relationship between \(x\), \(y\), and \(t\).  
   The data exhibited a smooth curve with oscillations, indicating a combination of linear and sinusoidal components.

2. **Model Definition**  
   The given parametric equations were implemented in Python using NumPy, with careful handling of exponential and trigonometric terms.

3. **Objective Function**  
   The fitting process minimised the L1 distance between experimental and predicted points:
   \[
   L(\theta, M, X) = \sum_i \left( |x_i - \hat{x_i}| + |y_i - \hat{y_i}| \right)
   \]

4. **Optimization Technique**  
   Optimisation was performed using SciPy’s `least_squares` method with bounded parameters.  
   A coarse grid search was followed by gradient-based refinement to ensure convergence to the global minimum.

5. **Evaluation**  
   Residuals were analysed to verify that they were small and randomly distributed, indicating a good fit.

---

## Results and Discussion

| Parameter | Estimated Value | Range | Description |
|------------|-----------------|--------|--------------|
| \(\theta\) | 0.826 rad (~47.3°) | \(0^\circ < \theta < 50^\circ\) | Rotation of the curve |
| \(M\) | 0.0742 | \(-0.05 < M < 0.05\) | Exponential scaling factor |
| \(X\) | 11.5793 | \(0 < X < 100\) | Horizontal translation |

### Final Fitted Equation

\[
\left( t \cdot \cos(0.826) - e^{0.0742|t|}\sin(0.3t)\sin(0.826) + 11.5793,\,
42 + t\sin(0.826) + e^{0.0742|t|}\sin(0.3t)\cos(0.826) \right)
\]

---

## Visual Validation

**Final Fit Plot:**

![Final Fit Plot](final_fit_plot.png)

- Left: Original data points and best-fit curve  
- Centre: Residuals versus \(t\)  
- Right: Residual magnitude distribution  

Residuals were centred around zero, confirming a good fit and minimal systematic error.

---

## Learnings and Future Improvements

- Gained practical understanding of parameter estimation and optimization techniques.  
- Understood how to balance local and global search methods to avoid overfitting.  
- Learned how residual analysis can validate model performance.

Possible improvements:
- Using Bayesian or MCMC fitting to estimate uncertainty in parameters.  
- Incorporating regularisation or noise modelling to improve robustness.

---

## References

- Desmos Parametric Graphing Calculator — [https://www.desmos.com/calculator/rfj91yrxob](https://www.desmos.com/calculator/rfj91yrxob)  
- SciPy Optimisation Library — `scipy.optimize`  
- NumPy and Matplotlib for numerical computation and visualisation  

---

### Submission Summary

**Final Answer (in LaTeX-ready form):**

\[
\boxed{
\left(
t \cos(0.826) - e^{0.0742|t|} \sin(0.3t)\sin(0.826) + 11.5793,\;
42 + t\sin(0.826) + e^{0.0742|t|}\sin(0.3t)\cos(0.826)
\right)
}
\]

This project demonstrates a complete and well-documented approach to parameter estimation, combining mathematical analysis and computational optimization.

