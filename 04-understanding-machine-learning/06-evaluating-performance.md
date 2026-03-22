# Evaluating Model Performance — A Developer's Guide

## Overview

Building a model is only half the job. You need to **measure how well it actually works** — and "well" means different things depending on your problem. This is Step 4 of the ML workflow.

---

## The #1 Problem to Watch For: Overfitting

Overfitting happens when your model **performs great on training data but poorly on new, unseen data**. It means the model memorized the training examples instead of learning generalizable patterns.

```
Overfit model:   100% accuracy on train set → 60% on test set  ❌
Good model:       95% accuracy on train set → 93% on test set  ✅
```

### Visual intuition

```
         Overfit (green line)          Good fit (black line)
         
  ● ●                                   ● ●
   ╰──╮  ●                              ──────────  ●
 ●    ╰──╯  ●                         ●          ●
```

The overfit line twists to perfectly classify every training point. It looks impressive on training data — but it will fail on any new data that doesn't follow that exact shape. The simpler black line makes a few more errors on training data but **generalizes much better**.

> This is exactly why we split data into train and test sets — to catch overfitting before deploying.

---

## Evaluating Classification Models

### Metric 1: Accuracy

The simplest metric. Percentage of predictions that were correct.

```
Accuracy = Correct Predictions / Total Predictions

Example: 48 correct out of 50 → 96% accuracy
```

### When accuracy fails: the fraud problem

Imagine a fraud detection model tested on 30 transactions where only 3 are fraudulent:

- Model predicts **everything as legitimate**
- Result: **90% accuracy** ← sounds good!
- Reality: **misses 100% of fraudsters** ← completely useless

This happens with **imbalanced datasets** — when one class is much rarer than the other. Accuracy becomes a misleading metric. You need something better.

---

## The Confusion Matrix

A confusion matrix breaks down predictions into 4 categories, giving you a complete picture of model performance.

```
                    Predicted: Fraud    Predicted: Legit
Actual: Fraud    |  True Positive (TP)  | False Negative (FN)  |
Actual: Legit    |  False Positive (FP) | True Negative (TN)   |
```

### The 4 cells explained

| Term | What it means | Real-world analogy |
|---|---|---|
| **True Positive (TP)** | Fraud correctly predicted as fraud | Smoke alarm goes off — there IS a fire ✅ |
| **True Negative (TN)** | Legit correctly predicted as legit | Smoke alarm is silent — no fire ✅ |
| **False Negative (FN)** | Fraud incorrectly predicted as legit | Smoke alarm silent — but there IS a fire ❌ |
| **False Positive (FP)** | Legit incorrectly predicted as fraud | Smoke alarm goes off — but no fire ❌ |

### Fraud example filled in

```
                    Predicted: Fraud    Predicted: Legit
Actual: Fraud    |       1 (TP)        |      2 (FN)         |
Actual: Legit    |       0 (FP)        |     27 (TN)         |

Total = 1 + 2 + 0 + 27 = 30 ✅
```

The matrix reveals what accuracy hid: the model only catches **1 out of 3 fraudulent transactions**.

> **Quick check:** all four cells should always sum to the total number of observations.

---

## Metric 2: Sensitivity (Recall)

Sensitivity measures how well the model catches **positive cases** (the thing you really care about finding).

```
Sensitivity = TP / (TP + FN)

Fraud example: 1 / (1 + 2) = 33%  ← very bad
```

### When to optimize for sensitivity

Use sensitivity when **missing a positive is the worst outcome**:

- 🏦 Fraud detection → missing a fraudster is costly
- 🏥 Cancer screening → missing a diagnosis is dangerous
- 🔒 Security threat detection → missing an intrusion is catastrophic

> With sensitivity, you'd rather have **false alarms** (flag legit things as suspicious) than **miss real threats**.

---

## Metric 3: Specificity

Specificity measures how well the model correctly identifies **negative cases**.

```
Specificity = TN / (TN + FP)
```

### When to optimize for specificity

Use specificity when **false alarms are the worst outcome**:

- 📧 Spam filters → sending a real email to spam is worse than letting spam through
- 🏥 Drug testing → falsely flagging a healthy patient as sick causes unnecessary treatment

> With specificity, you'd rather **let some bad things through** than constantly raise false alarms.

---

## Sensitivity vs. Specificity — The Trade-off

These two metrics are in tension. Optimizing one usually hurts the other.

| | Sensitivity | Specificity |
|---|---|---|
| **Optimizes for** | Catching all positives | Avoiding false alarms |
| **Tolerates** | False positives (false alarms) | False negatives (missed cases) |
| **Use when** | Missing a case is catastrophic | False alarms are costly |
| **Example** | Fraud detection, cancer screening | Spam filters, drug testing |

---

## Evaluating Regression Models

For regression, there's no "correct/incorrect" — the output is a number, so you measure **how far off the predictions are** from the actual values.

```
Error = Actual Value − Predicted Value

Goal: minimize the average error across all predictions
```

### Common regression metrics

| Metric | What it measures |
|---|---|
| **MAE** (Mean Absolute Error) | Average size of errors, easy to interpret |
| **RMSE** (Root Mean Square Error) | Penalizes large errors more heavily |

```
Example (apartment prices):
Actual:    $500k   $300k   $700k
Predicted: $480k   $320k   $650k
Errors:    $20k    $20k    $50k

MAE = ($20k + $20k + $50k) / 3 = ~$30k average error
```

---

## Evaluating Unsupervised Models

Unsupervised learning has **no labels and no correct output to compare against**, so you can't use accuracy, sensitivity, or RMSE.

Instead, you evaluate based on **how well the results serve your original goal**:

| Application | How you evaluate success |
|---|---|
| Customer clustering | Do the segments lead to better-performing marketing campaigns? |
| Anomaly detection | Are the flagged outliers actually meaningful anomalies? |
| Association rules | Do the discovered associations drive measurable lift in sales? |

> The evaluation is **business-driven**, not math-driven. Did the model help you achieve what you set out to do?

---

## Choosing the Right Metric — Quick Reference

| Problem type | Default metric | Use instead when... |
|---|---|---|
| Classification (balanced) | Accuracy | — |
| Classification (imbalanced) | Confusion matrix | Dataset has rare events (fraud, disease) |
| Catching rare events | Sensitivity | Missing a positive is costly |
| Avoiding false alarms | Specificity | False positives are costly |
| Regression | MAE or RMSE | — |
| Unsupervised | Business objective | — |

---

## Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Overfitting** | Model memorizes training data and fails to generalize to new data |
| **Accuracy** | % of predictions that were correct |
| **Confusion matrix** | Table breaking predictions into TP, TN, FP, FN |
| **True Positive (TP)** | Positive case correctly predicted as positive |
| **True Negative (TN)** | Negative case correctly predicted as negative |
| **False Positive (FP)** | Negative case incorrectly predicted as positive (false alarm) |
| **False Negative (FN)** | Positive case incorrectly predicted as negative (missed case) |
| **Sensitivity** | TP / (TP + FN) — ability to catch all positives |
| **Specificity** | TN / (TN + FP) — ability to avoid false alarms |
| **MAE** | Mean Absolute Error — average prediction error in regression |
| **RMSE** | Root Mean Square Error — penalizes large errors more heavily |

---

## Mental Model for Developers

```
Accuracy     → overall pass rate (good for balanced problems)
Sensitivity  → zero false negatives (catch everything, accept some noise)
Specificity  → zero false positives (no noise, accept some misses)
MAE/RMSE     → average distance from the correct answer (regression)
Unsupervised → did it actually help? (evaluate the outcome, not the math)
```

Think of sensitivity vs specificity like configuring alert thresholds in a monitoring system:
- **Low threshold** (high sensitivity) → catches every issue, but noisy with false alerts
- **High threshold** (high specificity) → clean alerts, but may miss real incidents

---

*Next up: How to improve model performance through tuning, feature engineering, and handling imbalanced datasets.*