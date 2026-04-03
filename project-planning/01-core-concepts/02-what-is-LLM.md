# What is an LLM?

- LLM is a part of machine learning field that process NLP (Natural Language Processing) - in other words, text.

- we can say this by looking at the name:

    Large: it requires a large amount of data and computational resources to function.
    Language: it is specialized in processing and analyzing human language data.
    Models: it is a capability of learning complex patterns from data.

- is this means that LLM only accepts text (PDF, TXT, articles)? No, not necessarily, it can accept other types of data, like images, audio, video, etc -> But this is no NLP

- when a model accepts not only text, it is called a "multimodal model" - in other words, it can process multiple types of data.

- The name "LLM" keeps the same cause it was created in a time when the only data accepted was text. But nowadays, it can process multiple types of data:

    Text  → NLP
    Image → Computer Vision
    Audio  → Speech processing

## Temperature in LLM

- temperature is a paremeter that controls if the model will generate creative/random or more deterministic/conservative responses

- it's a float number between 0 and 2 - this number depends of the model, there's model that accepts 0.0 and 1.0, and others that accepts 0.0 and 2.0

- default temperature mostly common is 1.0 - it depends of the model, check the documentation

0.0        → more factual answers, RAG and data extraction
0.3 - 0.7  → equilibrium between consistency and variation
0.7 - 1.0  → creative writing, brainstorming
1.0+       → experiments, rarely production


## Types of LLM models

1. LLM
- Most common, large, general purpose. Needs lots of data and compute
  to train. Good at zero-shot and few-shot prompting. Runs in the cloud
  because it's too big for local machines.
- Examples: GPT-4, Claude, Gemini

2. Fine-tuned Models
- Started as a large LLM, then trained further on specific domain data
  for a specific task. Smaller, faster, more precise for that task.
  Requires enough domain data to be worth fine-tuning.
- Examples: Codex (GPT-3 fine-tuned for code), InstructGPT

3. Edge Models
- Run directly on the device, no internet needed. Fast, private, zero
  cloud cost. Less capable than full LLMs.
- Examples: Phi-3, Google Translate offline, LLaMA 3 (small versions)
