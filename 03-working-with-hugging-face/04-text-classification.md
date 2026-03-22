# Text Classification — AI Lesson

## What is Text Classification?

Text classification is the task of assigning predefined labels to a piece of text based on its content. It's one of the most practical and widely used NLP techniques. Think of it as teaching a model to read text and decide: *"What category does this belong to?"*

---

## 1. Sentiment Analysis

The goal here is to detect the **emotional tone** of a text — positive, negative, or neutral.

| Input text | Label | Confidence |
|---|---|---|
| "I love pineapple on pizza" | Positive | 0.997 |
| "I dislike pineapple on pizza" | Negative | 0.993 |

### Code example

```python
from transformers import pipeline

classifier = pipeline("text-classification")
result = classifier("I dislike pineapple on pizza")
print(result)  # [{'label': 'NEGATIVE', 'score': 0.993}]
```

**Use cases:** product reviews, social media monitoring, customer feedback analysis.

---

## 2. Grammatical Correctness

This type checks whether a sentence is grammatically valid, labeling it as **Acceptable** or **Unacceptable**.

| Input text | Label | Meaning |
|---|---|---|
| "This course is great!" | LABEL_1 | Acceptable |
| "He eat pizza every day" | LABEL_0 | Unacceptable |

### Code example

```python
from transformers import pipeline

classifier = pipeline("text-classification", model="textattack/bert-base-uncased-CoLA")

result = classifier("He eat pizza every day")
# Output: LABEL_0 (Unacceptable) · score: 0.99
```

> `LABEL_0` = grammatically incorrect, `LABEL_1` = correct.

**Use cases:** grammar checkers, language learning apps, writing assistants.

---

## 3. Question Natural Language Inference (QNLI)

QNLI answers one specific question: *"Does this sentence actually answer this question?"* The model checks if a **premise** entails an answer to a given **question**.

**Question:** What state is Hollywood in?

| Premise | Result |
|---|---|
| "Hollywood is in California" | Entailment (True) |
| "Hollywood is known for movies" | Not Entailment (False) |

### Code example

```python
from transformers import pipeline

classifier = pipeline("text-classification", model="cross-encoder/qnli-electra-base")

result = classifier("Where is Seattle located?, Seattle is in Washington state")
# LABEL_0 = Entailment — the premise answers the question
```

> **Note:** You pass both the question and premise as a single string, **separated by a comma**.

**Use cases:** Q&A systems, fact-checking, search engines.

---

## 4. Dynamic Category Assignment (Zero-Shot Classification)

This is the most flexible type. You give the model a piece of text and a list of custom labels — and it assigns the best match, **even if it was never trained on those specific labels**.

### Code example

```python
from transformers import pipeline

classifier = pipeline("zero-shot-classification")

text = "Hey DataCamp, we'd like to feature your courses in our newsletter!"
labels = ["Marketing", "Sales", "Support"]

result = classifier(text, candidate_labels=labels)

# Access top result:
print(result["labels"][0])  # e.g., "Support"
print(result["scores"][0])  # e.g., 0.71
```

> The model picked "Support" over "Marketing" — it wasn't trained specifically for these labels, so results can be surprising. The key advantage is **you don't need a custom-trained model** for every new category set.

**Use cases:** content moderation, ticket routing, recommendation systems.

---

## Challenges of Text Classification

### Ambiguity
Text can have multiple meanings depending on context. Without contextual understanding, models can misclassify. For example: "I saw her duck" — is it an animal or a physical action?

### Sarcasm / Irony
Standard models often fail to detect tone inversion. "Oh great, another bug" uses technically positive words but carries negative sentiment.

### Multilingual Complexity
Portuguese, Arabic, and Mandarin have completely different grammatical structures. Models trained on English don't generalize well without specific multilingual training.

> Solving these challenges requires better preprocessing pipelines and models trained on more diverse and nuanced data.

---

## Quick Reference

| Type | Task | Output example |
|---|---|---|
| Sentiment Analysis | Emotional tone | Positive / Negative |
| Grammatical Correctness | Grammar check | Acceptable / Unacceptable |
| QNLI | Does premise answer question? | Entailment / Not Entailment |
| Zero-Shot Classification | Flexible category assignment | Sales · 0.87 |

---

> Text classification is a powerful entry point into NLP — once you understand the pipeline pattern, you can swap models and tasks with just a few lines of code.