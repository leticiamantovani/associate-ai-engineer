# Understanding the Need for External Context in Chatbots

When building a chatbot with a pre-trained language model, such as those from OpenAI, the model only knows what it has been trained on. This means it has a limited understanding based on the data it was trained with, which might not include the latest information or specific details relevant to your application.

## Why Is External Context Important?

There are two main reasons why a chatbot might lack certain information:

- **Knowledge Cut-off**:
The model's training data has a specific cut-off date. For example, if it was trained up to 2021, it won't know about events or data from 2022 onwards. If asked about recent topics, it might respond with a message indicating it doesn't have that information.

- **Limited Public Data**:
Some information isn't publicly available or wasn't included in the training data. For example, personal details about a user or proprietary company information won't be known to the model unless explicitly provided.

## How to Provide External Context

To make the chatbot more knowledgeable about specific topics or recent data, **you can supply additional context. There are two main methods**:

- **Sample Conversations**:

You can give the model examples of how it should respond in certain situations. For instance, showing a sample question and answer helps guide its behavior. However, this method can require many samples to cover different scenarios, which might be inefficient. (it will need user and assistant messages)

- **System Prompt (Initial Context)**:

A more effective approach is to **include the relevant information directly in the system prompt**. This prompt sets the stage for the conversation, defining the chatbot's role and providing key details. For example, if you want the chatbot to act as a customer service agent for a specific company, you can include the company's services and policies in the system prompt.

## Limitations of Context Size

It's important to note that **these methods work well for small to moderate amounts of information. Large contexts can exceed the model's capacity, which is limited by its token limit. When more extensive data is needed, advanced techniques are required, but these are outside the scope of this course.**

