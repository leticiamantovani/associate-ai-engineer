# Machine Learning Workflow — A Developer's Guide

## Overview

Building a machine learning model isn't just about feeding data into an algorithm. There are **four structured steps** that take you from raw data to a production-ready model.

```
Raw Data → [1. Extract Features] → [2. Split Dataset] → [3. Train Model] → [4. Evaluate]
                                                                                  ↓ (if not good enough)
                                                                              Tune & repeat from step 3
```

---

## The Scenario

Throughout this lesson, we'll use a real-world example to make each step concrete:

> **Goal:** Predict the sale price of apartments in New York City.

NYC releases monthly records of all apartment sales, including:
- Square footage
- Neighborhood
- Year built
- Sale price *(our target variable)*
- ...and more

Since the sale price is **already labeled** in the dataset, this is a **supervised learning** problem.

---

## Step 1 — Extract Features

### What is it?

Feature extraction is the process of preparing and selecting the input columns your model will learn from. Raw datasets are rarely ready to use out of the box — they need to be cleaned and reshaped.

You also need to **decide which features are relevant**. More isn't always better, but missing an important feature can hurt your model.

### NYC Apartment Example

Obvious features:
- `square_feet`
- `neighborhood`
- `year_built`

Non-obvious but potentially powerful features:
- `distance_to_nearest_subway` 🚇 ← easy to miss, but highly relevant in NYC!
- `floor_number`
- `number_of_rooms`

### Key point for developers

> Feature engineering is often **the most impactful part of the whole pipeline.** A simple model with great features beats a complex model with poor features almost every time.

---

## Step 2 — Split the Dataset

### What is it?

Before training, you split your full dataset into **two separate datasets**:

| Dataset | Purpose | Typical Size |
|---|---|---|
| **Train set** | The model learns from this data | ~70–80% of data |
| **Test set** | Used later to evaluate the model | ~20–30% of data |

### Why split?

You need a way to test whether your model actually **learned generalizable patterns** — not just memorized the training data. By holding out the test set during training, you guarantee the model has never seen that data when it's time to evaluate.

```
Full Dataset (100%)
    ├── Train Set (80%) → model learns from this
    └── Test Set  (20%) → model is evaluated on this (never seen during training)
```

> **Dev analogy:** Think of the train set as your codebase and the test set as your test suite. You don't write tests that just confirm what the code already does — you write tests that verify behavior on new inputs.

---

## Step 3 — Train the Model

### What is it?

The train dataset is fed into a **machine learning model** of your choice. The model iterates over the data and learns the relationships between features and the target.

### Choosing a model

There are many ML models, each with different strengths:

| Model | Best For |
|---|---|
| Linear Regression | Continuous numeric predictions (e.g., price) |
| Logistic Regression | Binary classification (e.g., spam / not spam) |
| Decision Tree | Interpretable classification/regression |
| Random Forest | High accuracy on tabular data |
| Neural Network | Complex patterns (images, text, audio) |

For our apartment price prediction, a model like **Linear Regression** or **Random Forest** would be a natural starting point.

### What happens during training?

```
Train Set (features + labels)
        ↓
  ML Model adjusts its internal parameters
        ↓
  Model learns: "when square_feet is high and neighborhood is Manhattan → price is high"
```

---

## Step 4 — Evaluate the Model

### What is it?

After training, you **can't just assume the model works well**. You need to measure its performance objectively using the test set — data the model has never seen.

### How evaluation works

```
Test Set (features only) → Model → Predictions
                                        ↓
                           Compare predictions vs. actual labels
                                        ↓
                              Calculate performance metric
```

### Common evaluation metrics

| Metric | Use Case | Example |
|---|---|---|
| Mean Absolute Error (MAE) | Average prediction error | "On average, we're off by $15,000" |
| Accuracy | Classification problems | "80% of predictions were correct" |
| % within margin | Business-friendly threshold | "78% of prices predicted within 10% of actual" |

### What counts as "good enough"?

You need to define a **performance threshold** before you start — this is a business/product decision, not a technical one.

Example thresholds for apartment price prediction:
- ✅ Good enough: predicting 80%+ of prices within a 10% margin
- ❌ Not good enough: average error of $100,000 on a $300,000 apartment

### What if the model isn't good enough?

You **tune the model** and go back to Step 3. Tuning can mean:

- 🔧 Adjusting model hyperparameters (e.g., learning rate, tree depth)
- ➕ Adding or removing features (go back to Step 1)
- 📦 Collecting more training data

> **Warning:** If performance isn't improving after multiple tuning attempts, it often means you simply **don't have enough data**. More data usually beats a smarter algorithm.

---

## The Full Workflow — Visual Summary

```
┌─────────────────────────────────────────────────────────────────┐
│  Step 1: Extract Features                                        │
│  Select and engineer input columns from raw data                 │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 2: Split Dataset                                           │
│  80% Train Set  |  20% Test Set                                  │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 3: Train Model                                             │
│  Feed train set into a chosen ML algorithm                       │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 4: Evaluate                                                │
│  Run test set through model → measure performance                │
│                                                                  │
│  Performance good enough? ──YES──→ ✅ Model is ready!           │
│         │                                                        │
│         NO                                                       │
│         ↓                                                        │
│  Tune model (adjust params, features, or get more data)         │
│         └──────────────────────→ Back to Step 3                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Feature extraction** | Selecting and preparing input columns for the model |
| **Train set** | The portion of data used to train the model (~80%) |
| **Test set** | Held-out data used only for final evaluation (~20%) |
| **Unseen data** | Another name for the test set — the model never saw it during training |
| **Evaluation metric** | A measurable score to assess model performance |
| **Performance threshold** | The minimum acceptable metric score to consider a model ready |
| **Tuning** | Adjusting model settings or features to improve performance |
| **Hyperparameters** | Model configuration options set before training (not learned from data) |

---

## Mental Model for Developers

```
Step 1 → Data preparation  (like schema design + data cleaning)
Step 2 → Test isolation    (like writing tests before shipping)
Step 3 → Build             (like compiling/deploying your code)
Step 4 → QA / CI           (like running your test suite — iterate until green)
```

If your "CI pipeline" (model evaluation) keeps failing, check your data first before reaching for a more complex algorithm.

---

*Next up: Deep dive into how specific supervised and unsupervised models work, and how to tune them effectively.*