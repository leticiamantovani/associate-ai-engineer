# Supervised Learning: Classification vs Regression — A Developer's Guide

## Overview

Supervised learning is essentially a **labeling machine** — it takes an input (observation) and assigns an output (label) to it. But not all labels are the same type, and that difference defines which flavor of supervised learning you need.

```
Supervised Learning
    ├── Classification  → output is a category    (discrete)
    └── Regression      → output is a number      (continuous)
```

---

## Classification

### What is it?

Classification assigns an observation to one of a **fixed set of categories**. The output is a **discrete variable** — meaning it can only be one of a few predefined values.

### Real-world examples

| Problem | Possible Labels |
|---|---|
| Customer churn prediction | `churns` / `stays` |
| Medical diagnosis | `cancerous` / `benign` |
| Wine type | `red` / `white` / `rosé` |
| Flower species | `rose` / `tulip` / `carnation` / `lily` |
| College admission | `accepted` / `rejected` |

### Anatomy of a classification problem

Using college admissions as an example:

- **Observations**: Each applicant
- **Features**: GPA, admission test score, extracurriculars, prizes won...
- **Target**: `accepted` or `rejected` ← only two possible values = classification

> When the target has exactly **two** possible values, it's called **binary classification**.  
> When it has three or more, it's called **multiclass classification** (e.g., the wine type example).

---

## Classification Models

### Support Vector Machine (SVM)

An SVM finds a **line (or curve) that separates classes** in the feature space. Despite the scary name, the core idea is simple: draw the best possible boundary between groups.

#### Linear SVM (straight line)

```
Test Score
    |          ● ● (accepted)
    |       ●  ●
    |   ────────────── ← decision boundary
    |  ○  ○
    |    ○  ○ (rejected)
    └──────────────── GPA
```

A straight line works reasonably well but can't perfectly separate non-linear data.

#### Polynomial SVM (curved line)

```
Test Score
    |       ● ●●
    |     ╭──────╮   ← curved boundary
    | ○  ○│ ● ●  │
    |    ○╰──────╯
    └──────────────── GPA
```

Allowing curves can achieve perfect (or near-perfect) separation — this is called **tuning** the model (more on that in later lessons).

### Key insight: more features = harder to visualize, easier for the model

With 2 features you can plot and eyeball the boundary. With 10, 50, or 500 features, humans can't visualize it anymore — but the model doesn't care. **Models scale effortlessly to high-dimensional data.**

---

## Regression

### What is it?

Regression assigns a **continuous numeric value** to an observation. The output can be any number — not just one of a fixed set of categories.

### Real-world examples

| Problem | Output |
|---|---|
| Stock price forecasting | `$142.87` |
| Exoplanet mass estimation | `3.2 Earth masses` |
| Child height prediction | `178.4 cm` |
| Apartment sale price | `$650,000` |
| Temperature prediction | `18.5 °C` |

### Example: Predicting temperature from humidity

- **Feature**: humidity (a number between 0 and 1)
- **Target**: temperature in °C (any number)
- **Observation**: the model finds the trend — as humidity rises, temperature decreases

A **linear regression** model fits a straight line through the data:

```
Temperature (°C)
    30 |  ●
    25 |    ●  ●
    20 |        ●──────── model line
    15 |             ●  ●
    10 |                 ●
       └──────────────────── Humidity
              0.2  0.5  0.8
```

Given humidity = `0.5` → model predicts temperature = `18.5 °C`

### Regression isn't perfect (yet)

The model catches the **general trend** but may still be inaccurate on individual predictions. Adding more features (wind speed, cloudiness, location, season) would improve accuracy significantly.

---

## Classification vs. Regression — Full Comparison

| | Classification | Regression |
|---|---|---|
| **Output type** | Discrete category | Continuous number |
| **Examples of output** | `spam` / `not spam` | `$18,500` |
| **Question it answers** | "Which group does this belong to?" | "How much / how many?" |
| **Common models** | SVM, Decision Tree, Logistic Regression | Linear Regression, Random Forest |
| **Evaluation metric** | Accuracy, F1-score | MAE, RMSE |

---

## You Get to Choose the Framing

This is a crucial insight: **the same problem can be framed as either classification or regression**, depending on what's more useful for your use case.

| Raw Problem | As Regression | As Classification |
|---|---|---|
| Temperature | `18.5 °C` (exact) | `Cold` / `Mild` / `Hot` |
| Age | `34 years old` | `Baby` / `Child` / `Teen` / `Adult` |
| Apartment price | `$645,000` | `Budget` / `Mid-range` / `Luxury` |
| Risk score | `0.87` | `Low risk` / `High risk` |

> **Rule of thumb:** If a business stakeholder would act differently based on exact numbers → use regression. If they just need to know which bucket something falls into → use classification.

---

## Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Classification** | Predicting which category an observation belongs to |
| **Regression** | Predicting a continuous numeric value |
| **Discrete variable** | A variable with a fixed set of possible values (categories) |
| **Continuous variable** | A variable that can take any numeric value |
| **Binary classification** | Classification with exactly two possible labels |
| **Multiclass classification** | Classification with three or more possible labels |
| **SVM (Support Vector Machine)** | A model that finds the best boundary line/curve to separate classes |
| **Linear regression** | A model that fits a straight line to predict a numeric output |
| **Decision boundary** | The line or curve that separates classes in a classification model |
| **Tuning** | Adjusting model behavior (e.g., allowing curves instead of straight lines) |

---

## Mental Model for Developers

```
Output is a category?   → Classification  (think: enum / union type)
Output is a number?     → Regression      (think: float / continuous value)
```

```typescript
// Classification → finite set of possible outputs
type AdmissionResult = "accepted" | "rejected";

// Regression → any numeric value
type PredictedTemperature = number;
```

---

*Next up: How to evaluate and tune classification and regression models, and how to choose the right algorithm for your problem.*