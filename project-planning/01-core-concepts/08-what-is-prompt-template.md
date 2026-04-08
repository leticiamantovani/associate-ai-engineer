# what is prompt template

- prompt template is a prompt with reusabke variables

- example:

```python
prompt = "You are a helpful assistant. Use the following context to answer the question.

Context: {context}

Question: {question}

Answer:
```

- this prompt has two variables and it will be replaced at runtime

## difference between prompt template and prompt

- prompt is the text with instructions

- prompt template is a prompt with reusable variables