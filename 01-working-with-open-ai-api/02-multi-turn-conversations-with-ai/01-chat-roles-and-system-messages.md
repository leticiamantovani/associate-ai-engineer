# Chat roles and system messages

Chat models from the OpenAI API can handle both single-turn and multi-turn interactions, allowing for a wide range of conversational applications.

## Single-turn vs. Multi-turn Tasks

 - **Single-turn tasks** involve a single input from the user and a single response from the model. Examples include generating text, transforming content, or performing classification tasks like sentiment analysis. These are straightforward interactions where the model responds based on one prompt.
 - **Multi-turn conversations** involve multiple messages exchanged between the user and the model. This setup maintains context across several exchanges, making interactions more natural and dynamic. For example, a chatbot that remembers previous questions and answers to provide coherent assistance.
 

## Roles in Chat Models

Roles are fundamental to how chat models interpret and generate responses. There are three main roles:

* **System role**: This message sets the behavior, tone, or rules for the assistant. It acts as **a guide** for how the model should respond throughout the conversation. For example, you might instruct the model to act as a polite Python tutor or a helpful customer service agent.
* **User role**: This contains the user's instructions, questions, or prompts. It represents **what the user is asking or instructing the assistant to do**.
* **Assistant role**: This is used for the model's responses. It can also be included in the message history as **an example** of how the assistant should reply, helping to steer the conversation.

## Structuring Conversations

To create a multi-turn conversation, you send a list of messages, each with a role and content. The sequence typically starts with a **system message** to set the context, followed by **user messages**, and optionally, **assistant messages** that serve as examples or responses. This structure allows the model to understand the context and respond appropriately.

## Using System Messages for Control and Safety

System messages are powerful tools to guide the assistant's behavior and prevent misuse. They can:

- **Define the assistant's role and behavior.**
- **Add restrictions or guardrails to prevent the model from generating undesired or harmful content.**
- **Ensure the assistant responds within specific boundaries, such as avoiding giving financial advice or sensitive information.**

For example, a system message might specify that the assistant is a **finance education helper** and should respond politely without providing specific investment advice. When a user asks for stock tips, the model will adhere to these instructions, responding accordingly.

