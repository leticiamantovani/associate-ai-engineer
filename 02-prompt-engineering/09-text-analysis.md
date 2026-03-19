# Text Analysis

Text analysis involves examining text data to extract useful information. It helps in understanding customer opinions, identifying key entities, and categorizing content. Two common techniques are text classification and entity extraction.

## Text Classification

### What it is:

Assigning predefined categories to a piece of text. For example, determining if a review is positive, negative, or neutral.

### How to do it:

Specify the categories explicitly in your prompt to guide the model. For example:

### Prompt example for sentiment analysis

```python
prompt = "Classify the sentiment of this review as positive, negative, or neutral: 'I love this smartwatch!' Respond with one word."
```
### Best practices:

- Always define the categories to get consistent results.
- If categories are not specified, the model relies on its internal knowledge, which may be less precise.

### Handling multiple emotions:

If a review expresses several feelings, instruct the model to list up to a certain number of emotions:

```python
prompt = "List up to three emotions expressed in this review: 'This phone is impressive but a bit expensive.'"
```

## Entity Extraction

### What it is:

Identifying specific pieces of information within text, such as names, dates, or product details.

### How to do it:

Specify the entities you want to extract and the format you prefer. For example:

### Prompt example for entity extraction

```python
prompt = """
Extract the following entities from the product description:
- Product name
- Display size
- Camera resolution

Format the answer as an unordered list.
Description: 'The new XYZ phone features a 6.5-inch display and a 108MP camera.'
"""
```

### Using few-shot prompts:


For complex structures, provide examples to guide the model:

### Example prompt with support tickets

```python
prompt = """
Extract entities from the following support tickets:

Ticket 1:
Customer: Alice
Phone: 123-456-7890
Issue: Cannot login

Ticket 2:
Customer: Bob
Reservation ID: 98765
Issue: Refund request

Now, extract entities from this new ticket:
Customer: David
Issue: Lost reservation
"""
```

### Practical Tips

- Always specify output format clearly.
- Use examples when the structure is complex.
- Define categories or entities explicitly to improve accuracy.
