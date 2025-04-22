# 🔍 Fairness-Aware Income Prediction

This project focuses on building a fairness-aware income prediction model using the UCI Adult Census dataset. It addresses biases related to gender and evaluates fairness using key metrics like **Disparate Impact (DI)** and **Statistical Parity Difference (SPD)**. A custom loss function with fairness penalty is also implemented to mitigate bias in the prediction.

---

## 📌 Table of Contents

- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Baseline Models](#baseline-models)
- [Bias Mitigation Techniques](#bias-mitigation-techniques)
- [Custom Fairness-Penalized Loss](#custom-fairness-penalized-loss)
- [Evaluation Metrics](#evaluation-metrics)
- [Results](#results)
- [How to Run](#how-to-run)
- [Folder Structure](#folder-structure)
- [Conclusion](#conclusion)

---

## 🎯 Problem Statement

The goal is to predict whether an individual earns more than $50K per year using features from the UCI Adult dataset, while ensuring the model is **fair with respect to gender (male vs. female)**.

---

## 📂 Dataset

- Source: UCI Adult Income Dataset
- Target variable: `income` (binary: `>50K` or `<=50K`)
- Sensitive attribute: `sex`

---

## 🤖 Baseline Models

We use:
- **Logistic Regression**: As a classical interpretable baseline
- **MLPClassifier**: For capturing nonlinearities and higher-order interactions

> Even though it's a classification problem, MLP is preferred over Linear Regression because it can better model the complex patterns in the data.

---

## 🧩 Bias Mitigation Techniques

1. **Balanced Sampling**  
   - Equal number of male and female samples in training set to reduce imbalance.

2. **Sample Weighting**  
   - Apply higher weights to disadvantaged group (e.g., females) in the loss function.

3. **Custom Fairness-Penalized Loss Function**  
   - Penalizes the model when disparity between groups increases during training.

---

## 🧮 Custom Fairness-Penalized Loss

We modify the loss function to include a penalty term:
Loss = CrossEntropy + alpha × FairnessPenalty

- `alpha` controls the trade-off between accuracy and fairness.
- Lower values of `alpha` reduce fairness penalty; higher values enforce fairness more strictly.

---

## 📊 Evaluation Metrics

- **Accuracy**: To evaluate prediction performance.
- **Disparate Impact (DI)**: Ratio of favorable outcomes between groups.
- **Statistical Parity Difference (SPD)**: Difference in favorable outcome rates.

> Ideal DI ≈ 1, SPD ≈ 0 (indicates fairness)

---

## ✅ Results Summary

| **Metric** | **Before Bias Mitigation** | **After Gender Column Removal** | **After Balancing Dataset** | **After Counterfactual Data Augmentation** | **After Adversarial Debiasing** | **After Fairness-Penalized** | **After Threshold Optimization (Equalized Odds)** | **After Threshold Optimization (Demographic Parity)** |
|------------|-----------------------------|----------------------------------|------------------------------|---------------------------------------------|-------------------------------|-----------------------------|----------------------------------------------------|--------------------------------------------------------|
| **Accuracy** | **0.8248** | **0.85** | **0.85** | **0.8801** | **0.8524** | **0.8433** | **0.8411** | **0.8346** |
| **Disparate Impact (DI)** | 0.35 | 0.35 | 0.37 | 1.03 | 0.52 | 0.74 | 0.57 | 1.01 |
| **Statistical Parity Difference (SPD)** | -0.20 | -0.20 | -0.19 | 0.01 | -0.10 | -0.05 | -0.09 | 0.00 |

> ⚖️ **Fairness Interpretation**:  
> • Ideal **Disparate Impact (DI)** ≈ 1  
> • Ideal **Statistical Parity Difference (SPD)** ≈ 0  
> 🎯 Some fairness interventions may slightly reduce accuracy but significantly enhance model fairness.

