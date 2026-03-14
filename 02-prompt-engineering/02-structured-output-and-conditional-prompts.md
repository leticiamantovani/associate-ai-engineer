# Structured output and conditional prompts

## Creating Structured Outputs in Prompts

To get the AI to produce outputs in specific formats like tables, lists, or formatted paragraphs, it is essential to explicitly mention the desired format in your prompt. For example, **if you want a table, specify the columns and the number of entries. For lists, clarify whether they should be numbered or unordered. When requesting paragraphs, describe the structure, such as including headings and subheadings. Clearly defining the output format helps the AI understand exactly what you need and produces more useful and organized results.

## Example

```python
prompt = "Create a table with columns 'title' and 'rating' for five action movies."
```

## Using Explicit Format Instructions
Always include instructions about the output format directly in your prompt. For example, you might say, "Create a table with columns 'title' and 'rating' for five action movies," or "Generate a list of the top five cities to visit, numbered." This explicit guidance ensures the AI's response matches your expectations and saves time in editing or re-prompting.

## Example

```python
prompt = "Generate a list of the top five cities to visit, numbered from 1 to 5."
```

## Handling Custom Output Formats

When you need a very specific output style, consider breaking down your prompt into parts. For example, first provide an input text, then instruct the AI to generate a title, and specify the format for each part. Combining these instructions in a single prompt helps the AI produce a cohesive, well-structured output that meets your needs.

### Why using triple backticks?
Triple backticks are used to delimit the input text. This helps the AI understand that the text is a code block and not part of the prompt.

```python
prompt = "Generate a title for the following text delimited by triple backticks: ```This is a test text```"
```

## Example

```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create the instructions
instructions = "Determine the language and generate a suitable title for the text delimited by triple backticks"

# Create the output format
output_format = "The output format should include text,language and title, each on a separate line, using 'Text:', 'Language:', and 'Title:' as prefixes for each line"

# Create the final prompt
prompt = f"""{instructions}{output_format}
```{text}```"""
response = get_response(prompt)
print(response)
```

## Conditional Prompts
Conditional prompts involve adding logic to your instructions, where the AI's response depends on certain conditions. For example, you can ask the AI to suggest a title only if the input text is in English or contains a specific keyword. If the condition is not met, the AI can respond with an alternative message, such as "I only understand English." Using conditions makes your prompts more flexible and context-aware, enabling more intelligent interactions.

## Example

```python
prompt = "Suggest a title for this text only if the text is in English. If it is in another language, reply with 'I only understand English'."
```

## Implementing Multiple Conditions
You can incorporate multiple conditions in a prompt. For example, instruct the AI to generate titles only if the text is in English and contains the word "technology." If these conditions are not satisfied, the AI can respond accordingly. This approach allows for more precise control over the output based on different scenarios.

## Example

```python
prompt = "If the input text is in English and contains the word 'technology', suggest a relevant title. Otherwise, reply with 'Conditions not met'"
```