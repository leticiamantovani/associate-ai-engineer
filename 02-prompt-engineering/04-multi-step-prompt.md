# Multi-step prompting

This technique involves **breaking down a complex goal into smaller, manageable steps**. Instead of asking the model to do everything at once, you guide it through a sequence of instructions. This helps improve accuracy and coherence, especially for tasks that require multiple actions or detailed reasoning.

## Example:
Suppose you want the model to write a travel blog about a trip to Paris. Instead of asking, "Write a travel blog about Paris," you can break it down:

First, ask the model to introduce Paris and its main attractions.
Then, request a description of personal experiences or adventures.
Finally, ask for a summary or conclusion about the trip.
This step-by-step approach ensures the output is well-structured and detailed.

## Benefits of multi-step prompting
Improves accuracy: By guiding the model through each part, you reduce the chance of irrelevant or incomplete responses.
Enhances coherence: The output follows a logical flow, making it easier to read and understand.
Handles complex tasks: Tasks like code review, problem-solving, or detailed explanations benefit from this method.

##Example:
When verifying Python code, instead of asking, "Is this code correct?" you can ask:

Step 1: Check if the syntax is correct.
Step 2: Verify if the code handles edge cases, like division by zero.

This way, the model provides a thorough evaluation.

## Difference between steps and shots
Steps: Explicit instructions that tell the model what to do at each stage. They act as a roadmap, guiding the process.
Shots: Examples of questions and answers that demonstrate how the model should respond. They serve as learning examples for the model.

## Example:

For **multi-step prompting**, steps might be:

"First, analyze the code syntax."
"Next, check for specific errors like division by zero."

While **shots** could be:

Q: "Is this code correct?"
A: "The syntax is correct, but it doesn't handle division by zero."

Both are important but serve different purposes.