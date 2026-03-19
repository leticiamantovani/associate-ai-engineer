# Introduction to Prompt Engineering for Chatbots

Prompt engineering is essential for optimizing how large language models (LLMs) like those used in chatbots perform. Since predicting user questions can be difficult, prompt engineering helps guide the chatbot's responses to be reliable and effective.

## Using the OpenAI API for Chatbots

In the OpenAI API, each message sent to the model has a role, such as "user," "assistant," or "system."

- The "user" role is for the **user's questions or inputs**.
- The "assistant" role is for the chatbot's responses **(an example of how the assistant should reply, helping to steer the conversation)**.
- The "system" role is used to **set the behavior and purpose of the chatbot**.

## Focusing on System Messages

System messages are crucial because they define how the chatbot should behave. For example, you can instruct the chatbot to **act as an expert data scientist who simplifies complex ideas**. When a user asks a question, the chatbot responds according to these instructions, ensuring consistent and appropriate answers.

### Example of System and User Messages

Suppose you want the chatbot to act as an expert data scientist. You send a system message like:

```python
"You are an expert data scientist who simplifies complex ideas."
```

Then, if the user asks, "Explain prompt engineering," the chatbot responds in a simplified manner, perhaps using an analogy like instructing a robot to prepare a sandwich.

## Modifying the Response Function

Previously, the function for getting responses handled only one prompt. Now, it needs to handle two: a system message and a user message. This means passing both messages to the function each time, so the chatbot knows its purpose and how to respond.

## Defining the Purpose and Behavior

The first thing to specify in a system message is the chatbot's purpose. For example, if building a financial chatbot, **you tell it to respond accurately, formally, and objectively about finance topics**. This helps the chatbot understand what questions to expect and how to answer them.

## Response Guidelines and Conditional Behavior

You can include guidelines in the system message **about the tone, length, and structure of responses**. For example, instruct the chatbot to answer only finance questions. If a user asks about the weather, the chatbot should respond with:

```python
"Sorry, I only know about finance."
```

This conditional behavior ensures the chatbot stays within its domain.