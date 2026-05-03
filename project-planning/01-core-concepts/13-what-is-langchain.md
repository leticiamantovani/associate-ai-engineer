# what is langchain

- langchain is a open source framework that provides a wrapper that you can construct agents with its functionalities

- you could write your code using the sdk from the ai as example, but if you connect with langchain, after that you could connect with another apps that will make these integrations easier

- another example is that you can use tools from sdk that facilitates the  business logic, such as example of chunking strategies, one of the most commons is the Text structure based that we use the RecursiveCharacterTextSplitter from langchain_text_splitters

## what is chains - important to know

chains = sequency

- it is literally a sequency of call executions - so when it comes the prompt, it calls the llm, and it returns to the user -> chain

- using langchain without langgraph is a LINEAR chain - cause it does not allow loops

## when using ai sdk from vercel instead of langchain

- focused in streaming answers to UI/web

- it integrates very well with next.js

- more simple and smoother

- using langchain we can use StreamingResponse from FastAPI and ReadableStream from browser NATIVE API
