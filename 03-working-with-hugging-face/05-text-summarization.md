# Summarization — AI Lesson

## What is Summarization?

Summarization is the process of reducing a large piece of text into a smaller one while retaining key information. It's one of the most useful NLP tasks, applied everywhere from news aggregators to legal document tools.

---

## Extractive vs. Abstractive Summarization

There are two main approaches, each with different trade-offs:

| | Extractive | Abstractive |
|---|---|---|
| **How it works** | Selects key sentences from the original text | Generates new text capturing the main ideas |
| **Output style** | Preserves original phrasing | Rephrases for clarity and readability |
| **Flexibility** | Low | High |
| **Resources needed** | Fewer | More computational power |
| **Risk** | May feel choppy or disconnected | May introduce fabrications |

### Extractive summarization
Picks the most important sentences directly from the source. Efficient and accurate — nothing is invented — but the result can feel less cohesive since sentences are lifted out of context.

### Abstractive summarization
Reads the text and rewrites it in a more natural, concise way. More readable, but the model may occasionally introduce information that wasn't in the original — a phenomenon known as **hallucination**.

---

## Use Cases

### Extractive summarization
Best when **accuracy is critical** and fabrication is unacceptable:

- **Legal document analysis** — highlighting key clauses from contracts or rulings
- **Financial research** — extracting main insights from earnings reports or filings

### Abstractive summarization
Best when **readability and engagement** matter more than verbatim accuracy:

- **News article summaries** — crafting concise, readable overviews for readers
- **Content recommendations** — generating compelling descriptions to drive clicks

---

## Extractive Summarization in Action

```python
from transformers import pipeline

# Choose a model designed for extractive summarization
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

text = """
Data science is an interdisciplinary field that uses scientific methods, processes,
algorithms, and systems to extract knowledge and insights from structured and
unstructured data. It combines statistics, mathematics, programming, and domain
expertise to analyze and interpret complex data...
"""

result = summarizer(text)
print(result[0]["summary_text"])

# primeiro_item = result[0]  # {'summary_text': 'Data science combines...'}
# texto = primeiro_item["summary_text"]  # 'Data science combines...'
# print(texto)
# Output: selects and returns key sentences from the original text
```

> The model selects key sentences from the input, producing a concise and fact-based summary **while preserving the original phrasing**.

---

## Abstractive Summarization in Action

```python
from transformers import pipeline

# DistilBART is designed specifically for abstractive summarization
summarizer = pipeline("summarization", model="sshleifer/distilbart-cnn-12-6")

text = """
Data science is an interdisciplinary field that uses scientific methods, processes,
algorithms, and systems to extract knowledge and insights from structured and
unstructured data...
"""

result = summarizer(text)
print(result[0]["summary_text"])
# Output: a newly generated, natural, and readable summary
```

> Unlike extractive summarization, this model **generates new sentences** — it doesn't copy from the source. The result reads more naturally but may occasionally include details not present in the original text.

---

## Controlling Summary Length with Parameters

You can control how long the generated summary is using two key parameters:

| Parameter | Description |
|---|---|
| `min_new_tokens` | Minimum number of tokens the summary must contain |
| `max_new_tokens` | Maximum number of tokens allowed in the summary |

### What is a token?
A token is a smaller unit of text that language models process — it can be a word, part of a word, or even a single character. For example, `"summarization"` might be split into `["summar", "ization"]`.

### Code example with length control

```python
summarizer = pipeline("summarization", model="sshleifer/distilbart-cnn-12-6")

result = summarizer(
    text,
    min_new_tokens=30,
    max_new_tokens=100
)

print(result[0]["summary_text"])
```

> These parameters ensure the summary is **concise, meaningful, and not overly verbose** — especially important when processing many documents in batch.

---

## Quick Reference

| Aspect | Extractive | Abstractive |
|---|---|---|
| Method | Selects original sentences | Generates new text |
| Accuracy | High (no fabrication) | Risk of hallucination |
| Readability | Can feel choppy | Natural and fluent |
| Best for | Legal, financial | News, recommendations |
| Example model | `facebook/bart-large-cnn` | `sshleifer/distilbart-cnn-12-6` |
| Key parameters | — | `min_new_tokens`, `max_new_tokens` |

---

> Summarization is a gateway to understanding how language models process meaning — not just words. Choosing the right approach depends on whether you prioritize accuracy or readability in your application.