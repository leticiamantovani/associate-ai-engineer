# what is streaming

streaming is a way to return live result as the llm thinks and returns answers - it returns the tokens in forms of chunks

- if you are using an api to do this, probably the framweork has a prop/function to help you integrate with that

- for fats aapi we have StreamingResponse as a type of response, once we implement that, we have to use yeld to return these chunks

- after that, we can use functions to determine the type of streaming that we want in langchain, examples:

## model

- model.stream() - this is used when you dont have anggraph with different agents yet - this will return chunks of text
- model.astream_event() - this will return chunks of events and etc

## agents

- for chunk in agent.stream(
    {"messages": [{"role": "user", "content": "What is the weather in SF?"}]},
    stream_mode=["updates", "custom"], 
    version="v2",
):

## notes

- notes that on agent we have different types of streaming - they are only available for agents

    messages: return the chunks
    updates: returns the result of each step of tool
    custom: returns something that it not in the state, for example, logs, metada etc