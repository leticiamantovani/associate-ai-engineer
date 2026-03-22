# Improving Model Performance — A Developer's Guide

## Overview

After evaluating your model, if the performance isn't good enough, you have options. This lesson covers three of the most common techniques to improve a model's results.

```
Three ways to improve performance:
    ├── 1. Dimensionality Reduction  → reduce the number of features
    ├── 2. Hyperparameter Tuning     → adjust the model's settings
    └── 3. Ensemble Methods          → combine multiple models
```

---

## 1. Dimensionality Reduction

### What is it?

"Dimension" = number of features in your dataset. Dimensionality reduction means **removing or consolidating features** to improve model performance.

### Wait — shouldn't more features = better predictions?

Not necessarily. Extra features can actually **hurt** your model by:
- Adding noise from irrelevant data
- Making the model overfit to patterns that don't generalize
- Slowing down training unnecessarily

### Three scenarios where you should reduce features

#### Scenario A: Irrelevant features

Some features simply have no relationship with the target and only add noise.

| Predicting commute time | Useful? |
|---|---|
| Time of day | ✅ Yes |
| Weather | ✅ Yes |
| Glasses of water drunk yesterday | ❌ No |

#### Scenario B: Highly correlated features

Some features carry almost the same information — keeping both is redundant.

```
Height  ←──── highly correlated ────→  Shoe size

Tall person → likely large shoe size
Short person → likely small shoe size
```

If you already have `height`, adding `shoe size` gives you almost no new information. **Keep one, drop the other.**

#### Scenario C: Collapsing features into one

Sometimes multiple features can be **merged into a single, more meaningful feature**.

```
height + weight  →  BMI (Body Mass Index)

Two features become one, with less noise and equal (or more) predictive power.
```

### Dev analogy

Think of it like database normalization — you don't want redundant columns carrying the same data. Lean schema = better queries. Lean feature set = better model.

---

## 2. Hyperparameter Tuning

### What is it?

A machine learning model has **settings you can configure before training** — these are called hyperparameters. Tuning them means finding the combination of values that produces the best performance for your specific dataset.

### The music console analogy

Think of a model like a music production console. The same console can mix pop, hip hop, or heavy metal — but each genre needs **different settings** for the guitar, bass, drums, and vocals.

```
Genre      = your dataset
Instruments = the model's components
Settings   = hyperparameters
```

Different datasets respond to different hyperparameter values. There's no universal "best setting."

### Real example: SVM kernel

Earlier we saw the SVM model use two different boundaries:

| Hyperparameter | Value | Result |
|---|---|---|
| `kernel` | `linear` | Straight line boundary |
| `kernel` | `polynomial` | Curved line boundary → better accuracy |

Switching the `kernel` hyperparameter from `linear` to `polynomial` was a tuning decision that improved performance.

### Key point for developers

You don't guess hyperparameter values randomly. There are structured search strategies (like **Grid Search** and **Random Search**) that systematically find the optimal combination — but that's a more advanced topic.

> Think of it like binary search vs. brute force: you can be systematic about finding the best config instead of guessing.

---

## 3. Ensemble Methods

### What is it?

Ensemble methods **combine multiple models** to produce a single, better-performing prediction. Instead of relying on one model, you let several models "vote" or "average" their way to a result.

The intuition: **a group of mediocre models working together often beats a single great model.**

### For Classification: Voting

Each model makes a prediction, and the **majority vote wins**.

```
Student admission example:

Model A → Accepted ✅
Model B → Rejected ❌
Model C → Accepted ✅

Majority vote → Accepted (2 out of 3)
```

### For Regression: Averaging

Each model predicts a number, and the **average is the final prediction**.

```
Temperature prediction example:

Model A → 5°C
Model B → 8°C
Model C → 4°C

Average → (5 + 8 + 4) / 3 = 5.67°C
```

### Why does this work?

Each model has its own blind spots and biases. By combining them, their individual errors tend to **cancel each other out**, leaving a more reliable result.

> Dev analogy: it's like code review from multiple reviewers. One reviewer might miss something, but three reviewers together are much less likely to all miss the same thing.

### Popular ensemble models you'll encounter

| Model | How it works |
|---|---|
| **Random Forest** | Combines many Decision Trees via voting |
| **Gradient Boosting** | Builds models sequentially, each fixing the previous one's errors |
| **XGBoost** | A highly optimized Gradient Boosting implementation — very popular in practice |

---

## Putting It All Together

These three techniques target different sources of underperformance:

| Technique | Fixes | When to use |
|---|---|---|
| **Dimensionality Reduction** | Too much noise / redundant features | Features are correlated or irrelevant |
| **Hyperparameter Tuning** | Model isn't configured well for your data | Default settings give mediocre results |
| **Ensemble Methods** | Single model has too many blind spots | You've tried individual models and want more accuracy |

They're not mutually exclusive — in practice, you often apply all three together.

---

## Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Dimensionality** | The number of features in your dataset |
| **Dimensionality reduction** | Removing or consolidating features to reduce noise and redundancy |
| **Correlated features** | Two or more features that carry similar information |
| **Hyperparameter** | A model setting configured before training (not learned from data) |
| **Hyperparameter tuning** | Finding the optimal hyperparameter values for your dataset |
| **Ensemble method** | Combining multiple models to produce a single better prediction |
| **Voting** | Ensemble strategy for classification — majority wins |
| **Averaging** | Ensemble strategy for regression — mean of all predictions |
| **Random Forest** | Popular ensemble model combining many Decision Trees |

---

## Mental Model for Developers

```
Dimensionality Reduction  → remove noise from input  (garbage in = garbage out)
Hyperparameter Tuning     → configure before compile  (like tuning build flags)
Ensemble Methods          → load balancing for models (spread the risk)
```

```typescript
// Single model — one point of failure
const prediction = modelA.predict(features);

// Ensemble — majority vote across models
const votes = [modelA, modelB, modelC].map(m => m.predict(features));
const prediction = majorityVote(votes);
```

---

*Next up: Practical workflows for applying these techniques and when to use each one in real projects.*