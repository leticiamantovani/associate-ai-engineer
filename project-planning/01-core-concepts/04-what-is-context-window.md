# what is context window

- context window is what LLM accepts in one request

- context window is measured in tokens

- each token is 0.75 words in English (can be more in Portuguese)

- this is your input, your convertion hisotry, your document and the output, all together

- each model has different context window, examples:

GPT-4o: 128k tokens
Claude Sonnet 4: 200k tokens

- if you reach the maximum of context, each model has a response different from this, OpenAI API returns an explicit error (context_length_exceeded), but frameworks like LangChain and LangGraph may silently truncate the oldest messages — no error, the model just forgets

- to give the model a conversation history, we do it manually, with arrays of inuput, but tools such as lanGraph can handle this better (but it also uses arrrays of input under the hood), example:

messages = [
    {"role": "system", "content": "Você é um assistente útil."},
    {"role": "user", "content": "Qual é a capital do Brasil?"},
    {"role": "assistant", "content": "A capital do Brasil é Brasília."},
    {"role": "user", "content": "E a capital da Argentina?"},  # nova pergunta
]

