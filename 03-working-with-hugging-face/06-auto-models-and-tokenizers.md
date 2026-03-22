# Auto Models and Tokenizers — AI Lesson

## Pipelines: Fast and Simple

Pipelines are a fantastic way to quickly perform tasks like text classification or summarization. But they abstract away all the details — which means **less control**. When you need to customize the process, you reach for **Auto Classes**.

---

## Auto Classes: Flexible and Powerful

Auto Classes are a flexible way to load models, tokenizers, and other components without manual setup. They give you full control over every step of the process.

| | Pipelines | Auto Classes |
|---|---|---|
| **Setup** | One line | A few steps |
| **Control** | Low | High |
| **Best for** | Quick experiments | Custom workflows |
| **Tokenizer pairing** | Automatic | Manual |

---

## AutoModels

To load a model directly, import the relevant `AutoModel` class for your task. Each task has its own class:

| Task | AutoModel class |
|---|---|
| Text classification | `AutoModelForSequenceClassification` |
| Text generation | `AutoModelForCausalLM` |
| Question answering | `AutoModelForQuestionAnswering` |
| Summarization | `AutoModelForSeq2SeqLM` |

### Code example

```python
from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
```

> `.from_pretrained()` downloads the model weights and configuration from the Hugging Face Hub using the model name.

---

## AutoTokenizers

A tokenizer converts raw text into a format the model can understand (numbers). It's important to use the **tokenizer paired with your model** — each model was trained with a specific tokenizer, and mixing them leads to poor results.

With pipelines, this pairing happens automatically. With Auto Classes, **you handle it yourself**.

### Loading a tokenizer

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
```

---

## Tokenizing Text

Tokenizers work in two steps:

1. **Cleaning** — lowercasing words, removing accents, normalizing whitespace
2. **Splitting** — dividing text into smaller chunks called **tokens**

### Code example

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")

tokens = tokenizer.tokenize("Summarization is a powerful NLP technique!")
print(tokens)
# ['summary', '##ization', 'is', 'a', 'powerful', 'nl', '##p', 'technique', '!']
```

> Notice how `"Summarization"` gets split into `["summary", "##ization"]` — the `##` prefix means it's a continuation of the previous token. **Different models produce different tokenizations for the same input.**

---

## Building a Custom Pipeline with Auto Classes

You can combine a model and tokenizer into a `pipeline()` manually, gaining full control over the process:

```python
from transformers import AutoModelForSequenceClassification, AutoTokenizer, pipeline

model_name = "distilbert-base-uncased-finetuned-sst-2-english"

model = AutoModelForSequenceClassification.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)

classifier = pipeline(
    task="sentiment-analysis",
    model=model,
    tokenizer=tokenizer
)

result = classifier("I love building AI applications!")
print(result)
# [{'label': 'POSITIVE', 'score': 0.9998}]
```

> By explicitly passing `model` and `tokenizer`, you control exactly which components are used — unlike the default `pipeline()` which picks them automatically.

Here it is:

The difference is the level of control, but the loaded model is the same either way.

Using pipeline with a model:

python
pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english")
The tokenizer is loaded automatically, inference happens under the hood, and you only receive the final result (label + score). Simple, but you have no access to intermediate tensors.

Using AutoModel + AutoTokenizer:

python
model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
You have access to everything — raw logits, attention tensors, embeddings. You decide what to do with the output.

The downloaded checkpoint is identical in both cases. What changes is what you can do after:

pipeline(model=...)	AutoModel
Loaded model	Same	Same
Tokenizer	Automatic	Manual
Output	label + score	Raw logits, tensors
Post-processing	Fixed	You control
Code	Less	More
Use pipeline when you want the final result quickly. Use AutoModel when you need to work with the model's raw output before reaching the final answer.

## TO USE AUTO TOKEN YOU SHOULD USE AUTO MODEL
## AUTO MODEL ALLOWS YOU TO HAVE MORE CONTROL ON PARAMS AND OUTPUTS

---

## When to Use Auto Classes Instead of Pipelines

Use AutoModels and AutoTokenizers when tasks demand more control:

### Advanced text preprocessing
Apply custom cleaning steps — domain-specific stopword removal, handling abbreviations, formatting — before tokenization.

```python
text = text.lower().replace("n/a", "not available")  # custom cleaning
tokens = tokenizer(text, return_tensors="pt")
```

### Custom thresholding in classification
Instead of accepting the model's top label, apply your own logic — for example, always routing to "Support" unless another category scores above 0.85.

```python
outputs = model(**tokens)
scores = outputs.logits.softmax(dim=-1)

if scores[0][support_index] > 0.4:
    label = "Support"
```

### Complex multi-step workflows
When your pipeline has multiple stages — preprocessing, encoding, classifying, post-processing — Auto Classes let you plug each piece in exactly where you need it.

## NOTES

When you specify a model in a pipeline:
```python
pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english")
```
**Hugging Face automatically loads the AutoTokenizer.from_pretrained(...) paired with that model — the weights, tokenization config, and vocabulary all come from the same checkpoint. This works because every model on the Hub has a config.json file that specifies which tokenizer to use. The pipeline reads that and handles the pairing automatically.** 

The difference only shows up when you don't specify a model:
```python
pipeline("sentiment-analysis")  # which model will it use?
```

In this case, Hugging Face picks a generic default for that task — and you lose control over which checkpoint is actually running, which is not ideal for production.

**Bottom line: if you explicitly pass a model name, you can trust that the default tokenizer will be the correct one for that model. If you don't, it falls back to a generic default.**

---

## Quick Reference

```python
from transformers import AutoModelForSequenceClassification, AutoTokenizer, pipeline

model_name = "distilbert-base-uncased-finetuned-sst-2-english"

# 1. Load tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_name)

# 2. Load model
model = AutoModelForSequenceClassification.from_pretrained(model_name)

# 3. Tokenize text
tokens = tokenizer.tokenize("Hello, world!")

# 4. Build custom pipeline
classifier = pipeline(task="sentiment-analysis", model=model, tokenizer=tokenizer)
result = classifier("Hello, world!")
```

---

> Auto Classes are the bridge between quick prototyping with pipelines and full production-grade control. Once you understand them, you can customize every layer of an NLP workflow.