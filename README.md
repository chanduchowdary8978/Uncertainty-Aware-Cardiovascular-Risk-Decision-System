# Uncertainty-Aware-Cardiovascular-Risk-Decision-System
An end-to-end ML system for cardiovascular risk prediction with calibrated probabilities, uncertainty estimation, and cost-sensitive decision-making.



# Uncertainty-Aware Cardiovascular Risk Decision System

This project builds an end-to-end machine learning pipeline to predict cardiovascular disease risk and convert predictions into actionable decisions. The focus is not only on model performance, but also on reliability, uncertainty, and decision-making under asymmetric risk.

---

## Problem

In clinical settings, standard binary classification is not sufficient:

- Missing a high-risk patient is more costly than a false alarm  
- Predicted probabilities must be reliable (well-calibrated)  
- Some predictions carry uncertainty and should not be acted on directly  

This project incorporates these considerations into the modeling pipeline.

---

## System Overview

The system performs the following:

1. Predicts probability of cardiovascular disease  
2. Calibrates probabilities to reflect true likelihood  
3. Estimates prediction uncertainty using bootstrap sampling  
4. Optimizes decision thresholds using cost-sensitive analysis  
5. Maps predictions into actionable risk categories  

---

## Modeling

- Models used: Logistic Regression, KNN, Random Forest, Gradient Boosting, LightGBM  
- Evaluation: Stratified cross-validation  
- Selection metric: ROC-AUC (~0.80 for best model)  

---

## Feature Engineering

Derived features:

- BMI = weight / height²  
- Pulse pressure = systolic − diastolic  
- Mean arterial pressure  

These features capture cardiovascular stress more effectively than raw inputs.

---

## Calibration

- Reliability curves used to compare predicted vs actual probabilities  
- Brier score used as calibration metric  

Ensures predicted probabilities can be interpreted as risk.

---

## Cost-Sensitive Decision Threshold

- Higher penalty assigned to false negatives  
- Threshold optimized using expected cost formulation  

Improves recall of high-risk patients.

---

## Uncertainty Estimation

- Bootstrap resampling used to estimate prediction variance  
- Confidence intervals computed for each prediction  

Policy:
- High risk + high uncertainty → defer decision  

---

## Robustness Testing

Model evaluated under simulated real-world conditions:

- Missing data  
- Measurement noise  
- Distribution shift (age-based subsets)  

This helps identify failure modes before deployment.

---

## Explainability

- SHAP values used for global and local interpretation  
- Verified that important features align with known clinical factors  

---

## Decision Policy

Final outputs are mapped into risk tiers:

- High Risk: immediate referral  
- Moderate Risk: monitoring and follow-up  
- Low Risk: routine check  
- Uncertain: defer decision  

---

## Results

- Best model: Gradient Boosting  
- AUC: ~0.80  
- Calibration improved probability reliability  
- Cost-sensitive threshold increased sensitivity  
- Performance degrades under high noise conditions  

---

## Limitations

- Dataset is partially synthetic  
- No longitudinal or time-to-event modeling  
- Bootstrap uncertainty is computationally expensive  
- Causal relationships are not established  

---

## Pipeline

```
Data → Preprocessing → Feature Engineering → Model → Calibration  
     → Uncertainty → Threshold Optimization → Decision Policy
```

---

## Tech Stack

- Python  
- NumPy, pandas, scikit-learn  
- LightGBM  
- SHAP  
- Matplotlib, Seaborn  

---

## Future Work

- Add experiment tracking (MLflow)  
- Deploy using FastAPI  
- Add monitoring for drift and calibration  
- Extend to survival analysis with longitudinal data  
