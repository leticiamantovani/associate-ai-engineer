# Introduction to AI and Machine Learning — AI Lesson

## Artificial Intelligence (AI)

Artificial Intelligence is a broad set of tools for making computers behave intelligently. It encompasses several sub-fields, including:

- **Robotics**
- **Machine Learning**
- **Computer Vision**
- **Natural Language Processing**

In recent decades, **machine learning has become the most prevalent subset of AI** — so when people say "AI" today, they're most likely referring to machine learning in practice.

---

## What is Machine Learning?

Machine learning is a **set of tools for making inferences and predictions from data**. Its methods are taken primarily from statistics and computer science.

The key idea: instead of being explicitly programmed with step-by-step instructions, **the computer learns patterns from existing data and applies them to new data**.

### Classic example — spam detection

```
1. Feed the model thousands of archived emails (spam and non-spam)
2. The model learns what spam looks like on its own
3. It then detects spam in new, unseen emails
```

No one programmed the rules for what makes an email spam — the model figured it out from the data.

---

## Inference vs. Prediction

Machine learning handles two types of tasks, which often work together:

| Task | Question it answers | Example |
|---|---|---|
| **Prediction** | What will happen? | Will it rain tomorrow? |
| **Inference** | Why did it happen? / What patterns exist? | Why does it rain? What weather types are there? |

> Inferences help make better predictions — but they require different types of machine learning models.

---

## How Does Machine Learning Work?

```
Existing data  →  Model learns patterns  →  Apply to new data  →  Output
```

For machine learning to be successful, it needs **high-quality data**. Garbage in, garbage out — the model is only as good as the data it learns from.

---

## Where Does Data Science Fit?

These terms are related but distinct:

| | Focus | Uses ML? |
|---|---|---|
| **Artificial Intelligence** | Making computers behave intelligently | Yes, ML is a subset |
| **Machine Learning** | Learning patterns from data to make predictions/inferences | Is the tool |
| **Data Science** | Discovering and communicating insights from data | Often, especially for predictions |

> Data science is about the insights. Machine learning is often the tool used to extract them.

---

## What is a Machine Learning Model?

A machine learning model is a **statistical representation of a real-world process** — like how we recognize cats, or how traffic changes throughout the day.

### How it works

```
Historical data  →  Train model  →  Input new data  →  Get output
```

### Example — traffic prediction

```python
# Conceptually:
model.train(historical_traffic_data)

# Later:
prediction = model.predict(date="tomorrow", time="3pm")
# Output: "Heavy traffic expected"
```

The output can also be a **probability**:

```python
# Example: fake tweet detection
result = model.predict(tweet="Breaking: aliens land in NYC!")
# Output: {'fake': 0.97, 'real': 0.03}
```

> A model essentially opens the "black box" between raw data and a useful answer — turning numbers into decisions.

---

## Quick Reference

| Concept | Definition |
|---|---|
| Artificial Intelligence | Broad field for making computers behave intelligently |
| Machine Learning | Subset of AI — learns patterns from data without explicit programming |
| Data Science | Discovering and communicating insights from data |
| Prediction | Forecasting the outcome of future events |
| Inference | Drawing insights — causes, patterns, behaviors |
| ML Model | Statistical representation of a real-world process |

---

> Machine learning is powerful because it removes the need for hand-crafted rules. Given enough high-quality data, the model figures out the rules itself — and that's what makes it so broadly applicable across medicine, marketing, finance, and beyond.