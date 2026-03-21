# Hugging Face Datasets — Complete Lesson

## 1. What are Hugging Face Datasets?

The Hugging Face Hub provides a large collection of community-curated datasets across a variety of tasks and domains (text, images, audio, etc.). Just like models, you can browse and filter them directly on the Hub under the **Datasets** tab.

---

## 2. Finding the Right Dataset on the Hub

Datasets can be filtered by:
- **Modality** — Text, Image, Audio, etc.
- **Task** — Text Generation, Classification, Translation, etc.
- **Keyword search** — to narrow results further

### Example: Finding an Italian Text Generation Dataset

1. Select modality → **Text**
2. Select task → **Text Generation**
3. Search keyword → `italian`

Each dataset has a **dataset card** containing:
- How the data was compiled
- License information
- Number of rows
- Data preview (Dataset Viewer)
- SQL querying via **Data Studio**

### Exploring with SQL (Data Studio)

```sql
-- Find rows containing the word "bella" (beautiful in Italian)
SELECT * FROM dataset
WHERE text LIKE '%bella%'
```

Once you're happy with the dataset, move to Python to start working with it.

---

## 3. Installing the `datasets` Package

Hugging Face provides a dedicated Python library for working with datasets.

```bash
pip install datasets
```

The `datasets` library lets you access, download, use, and share datasets with minimal code.

---

## 4. Downloading a Dataset

Use the `load_dataset()` function and provide the dataset path (found on the dataset card).

```python
from datasets import load_dataset

# Load the full dataset
dataset = load_dataset("path/to/dataset")

# Load only a specific split
train_data = load_dataset("path/to/dataset", split="train")
test_data  = load_dataset("path/to/dataset", split="test")
```

**Common splits:**
| Split | Purpose |
|---|---|
| `train` | Used to train the model |
| `test` | Used to evaluate the final model |
| `validation` | Used to tune and monitor training |

> Check the dataset card to confirm which splits are available.

---

## 5. Apache Arrow Format

Most Hugging Face datasets use **Apache Arrow** under the hood — a **columnar storage format** that is significantly faster for querying than traditional row-based formats (like CSV or pandas default storage).

You don't need to interact with Arrow directly — the `datasets` library handles it transparently.

**Columnar storage format** keeps all values of the same field together in memory. Same query reads only the age column — skipping everything else entirely. That's the speed advantage.

---

## 6. Data Manipulation

Manipulating Arrow datasets is slightly different from pandas DataFrames. The two main methods are `.filter()` and `.select()` (there are more).

### Filtering rows — `.filter()`

Uses a lambda function applied to each row. Returns rows where the condition is `True`.

```python
# Keep only rows where the text contains "bella"
filtered = dataset.filter(lambda row: "bella" in row["text"])
```

### Selecting rows by index — `.select()`

Returns a subset based on explicit indices.

```python
# Select the first two rows
subset = dataset.select(range(2))

# Access a specific row and column
print(subset[0]["text"])
```

### Summary of key methods

| Method | Purpose | Example |
|---|---|---|
| `.filter(fn)` | Keep rows matching a condition | `.filter(lambda x: "bella" in x["text"])` |
| `.select(indices)` | Pick rows by index | `.select(range(2))` |
| `dataset[i]["col"]` | Access a specific cell | `dataset[0]["text"]` |

---

## 7. Full Example — End to End

```python
from datasets import load_dataset

# 1. Load dataset
dataset = load_dataset("your-dataset-path", split="train")

# 2. Preview structure
print(dataset)
# Dataset({features: ['text', ...], num_rows: 10000})

# 3. Filter rows
filtered = dataset.filter(lambda row: "bella" in row["text"])

# 4. Select first 2 results
subset = filtered.select(range(2))

# 5. Read a specific entry
print(subset[0]["text"])
```

---

## 8. Key Takeaway

Hugging Face datasets follow the same discovery pattern as models — browse the Hub, apply filters, inspect the card, then load with a single function call. Once loaded, `.filter()` and `.select()` cover most data manipulation needs before you pass the data into a model.

---

## Resources

- [Hugging Face Datasets Hub](https://huggingface.co/datasets)
- [Datasets Library Docs](https://huggingface.co/docs/datasets/loading)
- [Processing Datasets (filter & select)](https://huggingface.co/docs/datasets/process#select-and-filter)
- [Apache Arrow Overview](https://arrow.apache.org/overview/)