# 2. What AI considers as token

## What are tokens?

Tokens are the building blocks of text that OpenAI models process. They can be as short as a single character or as long as a full word, depending on the language and context. Spaces, punctuation, and partial words all contribute to token counts. This is how the API internally segments your text before generating a response.

## Helpful rules of thumb for English:

1 token ≈ 4 characters

1 token ≈ ¾ of a word

100 tokens ≈ 75 words

1–2 sentences ≈ 30 tokens

1 paragraph ≈ 100 tokens

~1,500 words ≈ 2,048 tokens

Tokenization can vary by language. For example, “Cómo estás” (Spanish for “How are you”) contains 5 tokens for 10 characters. Non-English text often produces a higher token-to-character ratio, which can affect costs and limits.

## Examples

Here are some real-world text samples with their approximate token counts:

Wayne Gretzky’s quote “You miss 100% of the shots you don’t take” = 11 tokens

The OpenAI Charter = 476 tokens

The US Declaration of Independence = 1,695 tokens

## How token counts are calculated

When you send text to the API:

1. The text is split into tokens.

2. The model processes these tokens.

3. The response is generated as a sequence of tokens, then converted back to text.

4. Token usage is tracked in several categories:

Input tokens – tokens in your request.

Output tokens – tokens generated in the response.

Cached tokens – reused tokens in conversation history (often billed at a reduced rate).

Reasoning tokens – in some advanced models, extra “thinking steps” are included internally before producing the final output.

## Tokenization and f-strings in Python

Yes, spaces and line breaks **count as tokens** — so `"""` does affect cost.
```python
# Option 1 — with line breaks (more readable, more tokens)
prompt = f"""Summarize the following text into two bullet points:

{finance_text}

Be concise."""

# Option 2 — compact (fewer tokens, less readable)
prompt = f"Summarize the following text into two bullet points: {finance_text} Be concise."
```

In practice, the cost difference is **minimal** — a few line breaks are very few tokens. gpt-4o-mini is very cheap, so it's not worth sacrificing readability for it.

The general rule is:
- **Simple prompts** → one line is fine
- **Long, structured prompts** → use `"""` without worry, readability is worth it

What really impacts cost is the size of the content itself (`finance_text`, long instructions, examples) — not the line breaks in your code.

