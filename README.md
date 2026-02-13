# Black Box Optimization: AI/ML Capstone Project

## 1. Project Overview

The goal of this capstone project is to learn how to optimize unknown **black-box functions** using only a limited number of evaluated sample points. This setting closely reflects real-world challenges encountered in machine learning model tuning, where:
- The objective function is unknown  
- Function evaluations are expensive  
- Observations are sparse  
- Exhaustive search is infeasible  

Through this project, we explore practical techniques for **model-based optimization**, building intuition around:
- Surrogate modelling  
- Exploration–exploitation trade-offs  
- Sequential search strategies  

These concepts are fundamental to modern optimization workflows.

---

## 🎯 Problem Description

The project provides **eight synthetic black-box functions**, each designed to mimic real-world complexities such as:

- Non-linearity  
- Multiple local maxima  
- High-dimensional search spaces  
- Continuous inputs with floating-point precision  

Each function:

- Receives an array of continuous input parameters  
- Each parameter lies within the interval: 0.000000 ≤ x_i ≤ 1.000000
- Accepts values with up to **6 decimal places**
- Returns a **continuous output that must be maximized**
- Comes with an **initial dataset** of input points and corresponding outputs

### Initial Data Provided

| Function | Dimensionality | Initial Points |
|----------|---------------|----------------|
| f1       | 2D            | 10             |
| f2       | 2D            | 10             |
| f3       | 3D            | 15             |
| f4       | 4D            | 30             |
| f5       | 4D            | 20             |
| f6       | 5D            | 20             |
| f7       | 6D            | 30             |
| f8       | 8D            | 40             |

---

## 3. Optimization Objective

The objective is to **maximize each of the eight black-box functions** by proposing new query points.

This setup mirrors real-world constraints:

- Function evaluations are expensive  
- Only a limited number of queries are allowed  
- Strategic selection of evaluation points is required  
- Surrogate models must replace exhaustive search  

Success depends on balancing:

- **Exploration** → Sampling uncertain regions  
- **Exploitation** → Refining around high-value regions  

---

## 4. Optimization Methodology

A **Gaussian Process (GP)**–based surrogate model was used to guide the sequential optimization strategy for all eight functions.

The Gaussian Process provides:

- **Predicted mean (μ(x))** → Estimated function value  
- **Predictive variance (σ(x))** → Model uncertainty  

---

### Acquisition Strategy: Upper Confidence Bound (UCB)

The **Upper Confidence Bound (UCB)** acquisition function was used to select query points: UCB(x) = μ(x) + β · σ(x) 
Where:

- μ(x) → Predicted mean  
- σ(x) → Predictive uncertainty  
- β → Controls exploration vs. exploitation  

Higher β → More exploration  
Lower β → More exploitation  

---

### Iteration Strategy

#### 1️⃣ First Query Submission

- β was tuned to balance exploration and exploitation  
- UCB selected the next query point  
- The oracle evaluation result was added to the dataset  

---

#### 2️⃣ Second Query Submission

With updated knowledge:

- The new data point was added to the training set  

If the new point improved the best value so far:

- Exploit around this promising region  

Otherwise:

- Increase β to encourage exploration  

A new UCB-optimized query point was then submitted.

---

#### 3️⃣ Third Query Submission

Further refinement included:

- Centering the search space around the best-performing point  
- Constraining the search domain within `[0, 1]` for all parameters  

This narrowing strategy:

- Exploits local regions  
- Respects global parameter bounds  
- Improves efficiency under query limitations  

#### 4️⃣ Fourth Query Submission

Following the Week 3 submission, the F1 and F2 outputs did not improve, although they moved closer to the best results obtained so far. Based on this, I chose to continue exploitation in the next submission by reducing the exploration–exploitation parameter (β).

For F3 and F8, the outputs improved, which confirmed that exploitation was effective. As a result, I plan to further exploit the current region by narrowing and centering the search space around the best‑performing point.

The outputs of F4, F6, and F7 did not improve; however, the resulting values remained close to the current best, indicating that the previous query point is still the best found so far. This suggests that the search may be converging to a local maximum. Consequently, I will continue to narrow the search space while allowing limited exploration around the best point to potentially escape the local optimum.

A significant improvement was observed with F5, reinforcing the decision to prioritize exploitation going forward.

Up to this point, the Upper Confidence Bound (UCB) acquisition function has been used exclusively. In future iterations, I will also incorporate Expected Improvement (EI) to guide the selection of query points and better balance exploitation with targeted exploration.

The inputs associated with Function 5 appear to behave similarly to support vectors, as they tightly define the current decision boundary. This suggests that the next query should focus on exploitation in the vicinity of the best point identified so far. However, if subsequent query points fail to yield further improvements, the strategy will shift toward exploring other regions of the search space to identify alternative promising areas.

#### 5️⃣ Fifth Query Submission

Following the Week 4 submission, the output for F1 deteriorated, with the submitted query point performing well below the best output obtained so far. In response, I increased the emphasis on exploitation to focus the search more tightly around high‑performing regions.

For F2, the output showed a slight improvement, yielding a better query point overall. However, the results suggest that the search is operating near a local maximum. Consequently, I will continue prioritizing exploitation to refine the solution within this region.

The outputs for F3, F4, F7, and F8 deteriorated relative to the best point, but they remain within the top four results following the current best query. This indicates that these points lie within a promising region of the search space. As a result, I will maintain an exploitation‑driven strategy while selecting the next query point using the Expected Improvement (EI) acquisition function to allow for targeted local exploration.

Significant improvements were observed for both F5 and F6, with F5 in particular showing a substantial performance jump. Based on these results, I will continue to exploit the regions identified for both functions. The improvement observed for F5 is especially notable and can be likened, at a high level, to the breakthrough performance gains seen with AlexNet on the ImageNet classification task, where architectural and optimization choices led to a step change in performance. While it remains unclear whether the global optimum has already been reached, further refinement around this region is warranted.

---

## 5. Summary

This capstone project provides hands-on experience with:

- Black-box function optimization  
- Surrogate modelling using Gaussian Processes  
- Sequential exploration–exploitation strategies  
- Query-limited optimization scenarios  
- High-dimensional continuous search spaces  

The techniques applied here are foundational to:

- Hyperparameter optimization  
- Bayesian optimization  
- Model selection workflows  
- Real-world machine learning systems  

---

## 🔎 Key Takeaways

- Efficient optimization is possible without knowing the true function.
- Surrogate models enable intelligent decision-making.
- Exploration–exploitation balance is critical.
- Strategy matters more than brute force.
