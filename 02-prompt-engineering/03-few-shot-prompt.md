# Few Shot Prompt

## Introduction to Few-Shot Prompting
Few-shot prompting is a technique where you give the language model several examples within the prompt to guide its response. Instead of just asking a question, you include a few question-answer pairs that demonstrate the desired format or style. This helps the model understand exactly what kind of answer you're expecting.

## How Few-Shot Prompting Works
You prepare a prompt that contains multiple examples of questions and their correct answers. When you add your new question at the end, the model uses the patterns from the examples to generate an appropriate response. This approach is especially useful for complex tasks or when you want the output to follow a specific style.

## Example:

Suppose you want the model to convert numbers into sentences:

```python
messages=[
    {"role": "user", "content": "What is three plus five plus six?"},
    {"role": "assistant", "content": "Three plus five plus six equals 14."},
]
```

You provide this as an example in your prompt, and then ask:

```python
messages=[
    {"role": "user", "content": "What is three plus four plus seven?"},
]
```

The model will respond in a similar style, like:

```python
messages=[
    {"role": "assistant", "content": "Three plus four plus seven equals 14."},
]
```

## Benefits of Few-Shot Prompting
-   Guides the model to produce responses in a specific format.
- Improves accuracy for complex tasks by providing context.
- Reduces ambiguity by showing examples rather than just asking.

## When to Use Few-Shot Prompting

Use this technique when:
- The task is complex and needs multiple examples.
- You want the output to follow a particular style or structure.
- You have enough examples to cover all possible variations.
For simpler tasks, fewer examples or even zero-shot prompting (no examples) might be enough.

## Example of User and Assistant Role in Few-Shot Prompting
In practice, examples are often formatted as a conversation between a user and an assistant. Here’s how that looks:

```python
messages=[
    {"role": "user", "content": "What is three plus five plus six?"},
    {"role": "assistant", "content": "Three plus five plus six equals 14."},
    {"role": "user", "content": "What is three plus four plus seven?"}, 
    {"role": "assistant", "content": "Three plus four plus seven equals 14."},  
    {"role": "user", "content": "What is three plus four plus seven?"},
]

```
When you want the model to generate a response, you send the conversation as a prompt, and it continues in the same style.

## Choosing the Right Number of Examples

- Fewer shots -> better for simple tasks
- More shots -> better for complex tasks