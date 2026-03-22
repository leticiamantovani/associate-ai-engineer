# Unsupervised Learning — A Developer's Guide

## What is Unsupervised Learning?

Unsupervised learning is similar to supervised learning with one key difference: **there is no target column**. The model receives only features and must find patterns entirely on its own.

```
Supervised:    Features + Labels → model learns to predict labels
Unsupervised:  Features only     → model discovers hidden structure
```

This makes it especially powerful when you don't know much about your data yet — the model surfaces insights you didn't know to look for.

---

## Three Main Applications

```
Unsupervised Learning
    ├── Clustering          → find groups in the data
    ├── Anomaly Detection   → find outliers
    └── Association         → find relationships between events
```

---

## 1. Clustering

### What is it?

Clustering identifies **natural groups** in your dataset. Observations within a group are more similar to each other than to observations in other groups.

### The catch: the model doesn't explain itself

The model will find clusters — but **it won't tell you what those clusters mean**. That's your job as the developer/analyst.

**Example:** Given a dataset of 6 animals, the same algorithm could produce very different groupings depending on which features it weighs:

| Clustering result | Groups found |
|---|---|
| By species | Dogs vs. Cats |
| By color | Black / Grey / White / Brown |
| By origin | European breeds / Japanese breeds |

> In real life, you discover the clusters first, then investigate what they represent.

### Clustering Models

#### K-Means
- You **must specify the number of clusters (K) in advance**
- Fast and widely used for well-separated data
- Sensitive to your initial guess of K

```
K-Means with K=4 → finds 4 clusters
K-Means with K=3 → finds 3 clusters  ← (correct, if ground truth has 3 groups)
```

**Example:** Given flower measurements (petal width and length) with unknown species:
- With K=4 → produces 4 groups (one extra, incorrect)
- With K=3 → correctly separates Setosa, Virginica, Versicolor ✅

#### DBSCAN *(Density-Based Spatial Clustering of Applications with Noise)*
- You **don't need to specify the number of clusters in advance**
- Instead, you define what constitutes a cluster (e.g., minimum number of observations per cluster)
- Better at finding irregularly shaped clusters and handling noise/outliers

| | K-Means | DBSCAN |
|---|---|---|
| Number of clusters | You define upfront | Discovered automatically |
| Shape of clusters | Works best with round clusters | Handles any shape |
| Outlier handling | Assigns every point to a cluster | Can label points as noise |

---

## 2. Anomaly Detection

### What is it?

Anomaly detection finds **outliers** — observations that strongly differ from the rest of the data.

### Why not just eyeball it?

With 2 dimensions, you can spot outliers on a scatter plot. But:

```
2D  → easy to visualize ✅
3D  → doable with effort 😅
10D → impossible for humans ❌
100D → definitely need a model ❌
```

Unsupervised models handle hundreds of dimensions without breaking a sweat.

### Outliers aren't always errors

| Context | What the outlier means |
|---|---|
| Data cleaning | A total/sum row accidentally included in the dataset |
| Hardware monitoring | A device that fails much faster (or lasts much longer) than others |
| Fraud detection | A user whose behavior doesn't match normal patterns |
| Medical research | A patient who unexpectedly resists a fatal disease |

> Sometimes the outlier **is** the most interesting data point — not something to discard.

### Dev analogy

Think of anomaly detection like an alerting system. Normal traffic patterns = expected range. A sudden spike or drop = anomaly → trigger an alert. The model learns what "normal" looks like and flags anything that deviates significantly.

---

## 3. Association

### What is it?

Association finds **relationships between observations** — specifically, which events tend to happen together.

This is often called **market basket analysis**: *"Which products are bought together?"*

### Classic examples

| If a customer buys... | They are likely to also buy... |
|---|---|
| Jam | Bread |
| Beer | Peanuts |
| Wine | Cheese |
| Diapers | Baby wipes |
| Phone | Phone case |

### Real-world applications

- **E-commerce**: "Customers who bought X also bought Y" recommendations
- **Streaming**: "Users who watched X also watched Y"
- **Healthcare**: "Patients with condition A often also have condition B"
- **Retail**: Shelf placement and promotional bundling strategies

---

## Full Comparison: Supervised vs. Unsupervised

| | Supervised | Unsupervised |
|---|---|---|
| **Has labels?** | Yes | No |
| **Goal** | Predict a known output | Discover hidden structure |
| **Output** | Prediction (category or number) | Groups, anomalies, or associations |
| **You need to know** | What you're predicting | Almost nothing upfront |
| **Main techniques** | Classification, Regression | Clustering, Anomaly Detection, Association |

---

## Key Vocabulary Summary

| Term | Definition |
|---|---|
| **Clustering** | Grouping observations by similarity |
| **K-Means** | Clustering model requiring a predefined number of clusters (K) |
| **DBSCAN** | Clustering model that auto-discovers the number of clusters |
| **Outlier** | An observation that strongly differs from the rest |
| **Anomaly detection** | Identifying outliers automatically, especially in high dimensions |
| **Association** | Finding which events or items tend to co-occur |
| **Market basket analysis** | A common association technique to find products bought together |

---

## Mental Model for Developers

```
Clustering         → unsupervised groupBy()  (you don't define the key upfront)
Anomaly Detection  → automated alerting      (learn "normal", flag deviations)
Association        → dependency graph        (A and B tend to appear together)
```

```typescript
// Supervised: you define what to look for
const label = model.predict({ petalWidth: 1.2, petalLength: 3.4 }); // → "Setosa"

// Unsupervised clustering: model finds groups on its own
const cluster = model.assignCluster({ petalWidth: 1.2, petalLength: 3.4 }); // → "Cluster 2"
// What is Cluster 2? That's for you to investigate.
```

---

*Next up: How to evaluate unsupervised models and choose between clustering algorithms.*