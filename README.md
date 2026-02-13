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
