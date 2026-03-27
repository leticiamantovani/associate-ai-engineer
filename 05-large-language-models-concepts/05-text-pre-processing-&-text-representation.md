# 📝 NLP — Text Pre-processing & Text Representation
> A concise reference for developers getting into AI

---

## Where This Fits in the LLM Pipeline

```
Text Pre-processing  →  Text Representation  →  Pre-training  →  Fine-tuning
      ← You are here →
```

Before an LLM can learn anything, raw text must go through two major stages:
1. **Pre-processing** — clean and standardize the text
2. **Text Representation** — convert it into numbers the machine can read

---

## Stage 1: Text Pre-processing

Raw text is messy. Pre-processing transforms it into a clean, standardized format through three key steps. These steps are **independent** and can be applied in different orders depending on the task.

---

### Step 1 — Tokenization

**Splits text into individual units called tokens** (usually words or punctuation marks).

```
Input:  "Working with natural language processing techniques is tricky."

Output: ["Working", "with", "natural", "language", "processing", "techniques", "is", "tricky", "."]
```

> 💡 After tokenization, the sentence becomes a **list** — it's no longer a sentence, just a collection of tokens. Punctuation counts as its own token.

---

### Step 2 — Stop Word Removal

**Removes common words that carry little meaning** to focus on what actually matters.

Common stop words: `"with"`, `"is"`, `"the"`, `"a"`, `"and"`, `"of"` ...

```
Before: ["Working", "with", "natural", "language", "processing", "techniques", "is", "tricky"]

After:  ["Working", "natural", "language", "processing", "techniques", "tricky"]
```

> 💡 Stop word removal helps models focus on **signal**, not noise.

---

### Step 3 — Lemmatization

**Reduces words to their base (root) form**, so variations of the same word are treated as one.

```
"talking"  →  "talk"
"talked"   →  "talk"
"talks"    →  "talk"
```

Another example:
```
"running", "ran", "runs"  →  "run"
"better"                  →  "good"
```

> 💡 Lemmatization prevents the model from treating `"talked"` and `"talk"` as two completely different concepts — they're the same idea.

---

## Stage 2: Text Representation

Once the text is clean, it needs to be converted into **numbers** — the only language computers truly understand. There are two main techniques:

---

### Technique 1 — Bag of Words (BoW)

Creates a **matrix of word counts** across sentences/documents.

**Example:**

| | cat | chased | mouse | swiftly |
|---|---|---|---|---|
| "The cat chased the mouse swiftly" | 1 | 1 | 1 | 1 |
| "The mouse chased the cat" | 1 | 1 | 1 | 0 |

Stop words (`"the"`) are removed first. Each unique word becomes a column. `1` = present, `0` = absent.

**❌ Limitations of Bag of Words:**

1. **No context or meaning** — "The cat chased the mouse" and "The mouse chased the cat" produce very similar matrices, but mean the opposite.
2. **No semantic relationships** — "cat" and "mouse" are treated as completely unrelated, even though they are predator and prey.
3. **No word order** — the "bag" metaphor is literal: words are just thrown in with no regard for sequence.

---

### Technique 2 — Word Embeddings

**Represents words as vectors (lists of numbers)** that capture their *meaning and semantic relationships*.

```
"cat"   →  [-0.9,  0.9,  0.9]   (high carnivore score, high furry score, low plant score)
"tiger" →  [-0.8,  0.95, 0.85]  (similar to cat → similar vector)
"mouse" →  [ 0.1,  0.6,  0.0]   (prey, different from cat)
```

> 💡 The feature labels (like "carnivore", "furry", "plant") are **not manually defined** — the model learns them automatically from training data. In practice, you'll only see raw numbers.

**Why this is powerful:**

| Pair | Relationship captured |
|---|---|
| cat / tiger | Both predators → similar vectors |
| mouse / rabbit | Both prey → similar vectors |
| eagle / rabbit | Predator-prey → similar relationship to cat-mouse |

Similar meaning = vectors that are **close to each other** in mathematical space. This is what allows LLMs to understand that "king - man + woman ≈ queen".

---

## Full Pipeline — Putting It All Together

```
Raw text
   ↓
Tokenization        →  Split into tokens
   ↓
Stop word removal   →  Remove low-value words
   ↓
Lemmatization       →  Reduce to root forms
   ↓
Bag of Words        →  Simple word count matrix (limited)
        OR
Word Embeddings     →  Rich numerical vectors with meaning (preferred for LLMs)
   ↓
Machine-readable numbers  →  Ready for model training
```

---

## Summary

| Technique | What it does | Limitation |
|---|---|---|
| **Tokenization** | Splits text into tokens | — |
| **Stop word removal** | Removes meaningless common words | — |
| **Lemmatization** | Reduces words to base form | — |
| **Bag of Words** | Word count matrix | No context or semantic meaning |
| **Word Embeddings** | Vectors capturing meaning | More complex, but far superior |

---