# 1. Differences between names


When working with OpenAI API, it's important to understand the differences between the names of the parameters and the names of the arguments.

## OpenAI - what is this?

open ai is a company that provides a set of tools for building applications that use artificial intelligence.


## chatgpt - what is this?

chatgpt is a chatbot that uses the openai api to answer questions and help with tasks.


## openai api - what is this?

openai api is a set of tools for building applications that use artificial intelligence.


## openai api key - what is this?

openai api key is a key that is used to authenticate requests to the openai api.


## openai api url - what is this?

openai api url is the url of the openai api.


## openai api version - what is this?

openai api version is the version of the openai api.


## parameters to make a request to the openai api 

parameters to make a request to the openai api are the parameters that are used to make a request to the openai api.

role - the role of the user or the assistant
content - the content of the message


## arguments to make a request to the openai api

arguments to make a request to the openai api are the arguments that are used to make a request to the openai api.

model - the model to use
temperature - the temperature to use
max_tokens - the maximum number of tokens to generate

## Example

```python
import openai

client = openai.OpenAI(api_key="your_api_key")

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "Hello, how are you?"}],
    temperature=0.5,
    max_tokens=100
)
```