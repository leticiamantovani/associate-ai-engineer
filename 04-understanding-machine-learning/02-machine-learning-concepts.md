# Machine Learning Types — A Developer's Guide

## 1. The Three Types of Machine Learning

Machine learning is broadly divided into three types:

| Type | Main Use Case | Complexity |
|---|---|---|
| **Supervised Learning** | Predict outcomes from labeled data | Medium |
| **Unsupervised Learning** | Find patterns in unlabeled data | Medium |
| **Reinforcement Learning** | Sequential decision-making (e.g., games, robotics) | High |

> **Note for developers:** Reinforcement learning uses complex math (like game theory) and is much less common in everyday applications. This guide focuses on supervised and unsupervised learning — the two you'll encounter most.

---

## 2. Training Data — The Foundation of Everything

Before a model can make predictions, it needs to **learn from existing data**. This existing data is called **training data**, and the process of the model learning from it is called **training a model**.

```
Training Time: nanoseconds → weeks (depends on data size and model complexity)
```

Think of it like teaching a junior developer by showing them past projects — the more examples (data) you provide, the better they learn.

---

## 3. Supervised Learning

### What is it?

In supervised learning, the training data comes with **labels** — meaning you already know the correct answer for each example. The model learns the relationship between inputs (features) and the known output (label), so it can predict the output for new, unseen inputs.

### Anatomy of a Supervised Learning Dataset

Using a heart disease prediction example:

| Age | Cholesterol | Smoker | **Heart Disease** ← *target* |
|-----|-------------|--------|-------------------------------|
| 45  | 210         | Yes    | **True**                      |
| 30  | 170         | No     | **False**                     |
| 60  | 280         | Yes    | **True**                      |

- **Target variable**: What you want to predict (`Heart Disease`)
- **Labels**: The known values of the target (`True` / `False`, or numbers, or categories)
- **Features**: Input columns used to make the prediction (`Age`, `Cholesterol`, `Smoker`)
- **Observations/Examples**: Each row — more rows = better model

### How it works (step by step)

```
1. Feed labeled data (features + labels) → model learns patterns
2. Give model new data (features only, no label)
3. Model outputs its prediction
```

### Real-world examples

| Problem | Features | Label |
|---|---|---|
| Email spam detection | Word frequency, sender, links | Spam / Not Spam |
| House price prediction | Size, location, rooms | Price (number) |
| Credit card fraud | Amount, location, time | Fraud / Legit |
| Heart disease | Age, cholesterol, smoking | True / False |

### Code analogy

```typescript
// Think of a supervised learning model like a trained function:
function predictHeartDisease(age: number, cholesterol: number, isSmoker: boolean): boolean {
  // The model "learned" what goes inside here from training data
  return model.predict({ age, cholesterol, isSmoker });
}
```

---

## 4. Unsupervised Learning

### What is it?

In unsupervised learning, there are **no labels** — you only have features. Instead of predicting a known outcome, the model finds its own patterns and structure in the data.

The two most common tasks are:
- **Clustering** — grouping similar data points together
- **Anomaly detection** — finding outliers

### How it works (step by step)

```
1. Feed unlabeled data (features only) → model finds hidden patterns
2. Model outputs groups/clusters or anomaly scores
3. You discover patterns you didn't know existed beforehand
```

### Example: Patient Clustering

Using the same heart disease dataset, but this time **without labels**:

| Age | Cholesterol | Blood Sugar |
|-----|-------------|-------------|
| 55  | 290         | High        |
| 57  | 310         | High        |
| 32  | 160         | Normal      |
| 30  | 150         | Normal      |

A clustering model might automatically discover two groups:
- **Cluster A**: Older patients with high cholesterol and blood sugar
- **Cluster B**: Younger patients with normal values

> You didn't tell the model these groups existed — it figured them out on its own.

### Why use unsupervised learning?

Because **labeling data is expensive and sometimes impossible**:

- 🚗 Self-driving cars: Manually labeling millions of road images is impractical
- 🛒 E-commerce: Grouping customers by behavior without predefined segments
- 🔐 Security: Detecting unusual network traffic patterns (anomalies) with no prior examples

### Real-world examples

| Problem | Features | Output |
|---|---|---|
| Customer segmentation | Purchase history, demographics | Customer groups |
| Network intrusion detection | Traffic patterns | Normal / Anomaly |
| Document grouping | Word frequency | Topic clusters |
| Product recommendations | Browsing/purchase behavior | Similar user groups |

---

## 5. Supervised vs. Unsupervised — Quick Comparison

| | Supervised | Unsupervised |
|---|---|---|
| **Training data** | Labeled (you know the answers) | Unlabeled (no predefined answers) |
| **Goal** | Predict a known output | Discover hidden patterns |
| **Output** | Prediction (category or number) | Groups, clusters, anomalies |
| **Human effort** | High (labeling required) | Low (no labeling needed) |
| **Common tasks** | Classification, Regression | Clustering, Anomaly Detection |
| **Example** | "Does this patient have heart disease?" | "What types of patients do we have?" |

---

## 6. Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Training data** | Existing data used to teach the model |
| **Training a model** | The process of the model learning from training data |
| **Target variable** | The output the model is trying to predict |
| **Labels** | Known values of the target variable in training data |
| **Features** | Input columns/attributes used to make predictions |
| **Observations** | Individual rows/examples in the dataset |
| **Clustering** | Grouping data points by similarity (unsupervised) |
| **Anomaly detection** | Finding outliers or unusual patterns (unsupervised) |

---

## 7. Mental Model for Developers

```
Supervised   → You have X and Y in training. You predict Y for new X.
Unsupervised → You only have X. You find structure within X.
```

Or as a code metaphor:

```typescript
// Supervised: you provide input AND expected output during training
const trainingData = [
  { input: { age: 45, cholesterol: 210 }, output: true },
  { input: { age: 30, cholesterol: 170 }, output: false },
];

// Unsupervised: you only provide input, let the model find groups
const rawData = [
  { age: 45, cholesterol: 210 },
  { age: 30, cholesterol: 170 },
];
```

---

*Next up: Deep dive into how supervised and unsupervised learning work internally, and specific use cases.*