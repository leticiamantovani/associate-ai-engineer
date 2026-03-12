# 4. Text generation

- When we give a text to the model, it will not always generate the same text, that's where randomness comes in.
- The answers will be **undeterministic**.

## Temperature

- Temperature is a parameter that controls the randomness of the text generation.
- It's a float number between 0 and 2.
- The higher the temperature, the more random the text will be.
- The lower the temperature, the more predictable the text will be.
- The default temperature is 1.0.

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Hello, how are you?"}],
    temperature=0.5
)
```