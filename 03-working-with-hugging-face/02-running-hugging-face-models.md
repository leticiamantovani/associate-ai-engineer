# Running Hugging Face Models — Inference Lesson

## 1. What is Inference?

Inference is the act of using a trained model to make predictions. When you "run a model", you're doing inference — you give it input, it processes, and returns an output.

---

## 2. Two Ways to Run Inference

You can either run models **locally** (on your own machine or cloud environment) or delegate that work to an **inference provider** — partner companies that give you access to high-performance hardware (especially GPUs) via API.

| | Local | Inference Provider |
|---|---|---|
| Cost | Free | Free credits, then paid |
| Speed | Slow on large models | Fast (GPU) |
| Setup | Simple | Needs HF token |
| Best for | Small models, experimentation | Large LLMs, production |

---

## 3. The `pipeline` — Local Inference Made Simple

The `pipeline` class from Hugging Face `transformers` is your shortcut to running any model locally. You pick a task and a model, and it handles everything else (tokenization, model loading, post-processing).

```python
from transformers import pipeline

# Create a text generation pipeline using GPT-2
generator = pipeline("text-generation", model="openai-community/gpt2")

# Run inference
result = generator("What if AI", max_new_tokens=10, num_return_sequences=2)

# Result is a list of dicts
for item in result:
    print(item["generated_text"])
```

**Key parameters:**
- `max_new_tokens` — limits the output length
- `num_return_sequences` — generates multiple completions

The output is always a list of dictionaries with a `generated_text` key.

---

## 4. Inference Providers — Remote GPU Power

When local hardware isn't enough (think large LLMs or image generation), you offload the computation to a provider. You send your input via API, they run the model on fast GPUs, and return the result.

```python
from huggingface_hub import InferenceClient

# Connect to an inference provider
client = InferenceClient(
    provider="together",        # inference provider name
    api_key="YOUR_HF_TOKEN"     # your Hugging Face token
)

# Chat-style input (most modern LLMs use this format)
messages = [
    {"role": "user", "content": "Explain neural networks in one sentence."}
]

response = client.chat.completions.create(
    model="meta-llama/Llama-3-8b-instruct",
    messages=messages
)

print(response.choices[0].message.content)
```

**The messages format** (list of dicts with `role` and `content`) is the standard way to prompt conversational models — the same pattern used by OpenAI, Anthropic, and others.

---

## 5. Key Takeaway

Hugging Face gives you a unified interface — whether you run things locally with `pipeline` or remotely via `InferenceClient`, the workflow stays the same:

1. Pick a model
2. Provide input
3. Get output

The infrastructure is what changes under the hood.

---

## Resources

- [Hugging Face Transformers](https://github.com/huggingface/transformers)
- [Inference Providers Docs](https://huggingface.co/docs/inference-providers/en/index)
- [GPT-2 Model Card](https://huggingface.co/openai-community/gpt2)