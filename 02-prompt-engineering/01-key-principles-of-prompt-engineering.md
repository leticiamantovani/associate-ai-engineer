# Key Principles of Prompt Engineering

In this section, we focus on how to create effective prompts to get the best results from a language model. The goal is to guide the model clearly and precisely, much like giving directions to someone to reach a specific destination efficiently.

## 1. Use of Action Verbs
When designing prompts, **choose strong, explicit action verbs** such as **write, explain, describe, evaluate, or summarize**. These verbs tell the model exactly what kind of output you expect.

Example:

Instead of asking, "Tell me about deforestation," ask, "Propose strategies to reduce deforestation."

This directs the model to perform a specific task rather than giving a vague request.
Avoid ambiguous verbs like **understand, think, feel, try, or know**, as they can confuse the model about what you want it to do.

## 2. Providing Detailed and Precise Instructions
Be specific about the context, style, length, and audience for the output. **The more detailed your instructions, the better the model can tailor its response**.

Example:

Instead of asking, "Tell me about dogs," specify, "Describe the behavior and characteristics of Golden Retrievers, including their suitability for families."
This results in a more focused and informative answer.

## 3. Structuring the Output
Specify the **desired structure of the response, such as the number of paragraphs, sentences, or words.** This helps control the length and format of the output**.

Note:
While you can limit output length using parameters like max_tokens, explicitly instructing the model within the prompt ensures more control and completeness.

## 4. Combining Instructions with Input Data

Prompts often include instructions along with input data. To keep prompts organized, **place instructions at the beginning**, then delimit input data with clear markers like parentheses, brackets, or backticks.

Example:

Summarize the following text:
[Input text here]

Using delimiters helps the model identify the input to operate on.

## 5. Embedding Variables with Python F-Strings

When working programmatically, **embed input data into prompts using Python's f-strings**. This allows **dynamic input data to be used in the prompt**.

Example:

```python
prompt = f"Summarize the following text: {input_text}"
```