# what is short and long memory

when we are using a model, it's important that this model remembers older conversation to have more context to have better answers

the problem is that this this conversation consumes context window so we need to store this conversation in a database or in a file

today we have two types of memory

- short memory

short memory is the conversation that the model sees in the current prompt

- long memory

long memory is the memory that is stored in a database or in a file. this memory persists between sessions, this memory becames a short memory when it is inserted again in the prompt

## what should i do when the memory doesnt fit on the context window?

- sliding window

only passes the last N messages to the model, but it loses older messages

- summarization

when the history becames too long, we use a model to summarize the history and pass to the model, it loses details but it fits on the context window

- hybrid approach

we use a model to summarize older messages and we also send the last N messages to the model

- RAG 

we embed the older messages and stores them in a vector db, we get the most relevant messages, usually we use pure text to be embedded and we get the most relevant messages, its considered rag cause it retrieves something older to put in the context window

we use conversatio_id to retrieve the most important messages from that conversation or user_id to get important messages from that user that we must remember