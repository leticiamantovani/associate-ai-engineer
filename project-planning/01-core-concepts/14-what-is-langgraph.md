# what is langgraph

- langgraph is a open source framework that you can execute nodes and tools, it also has observability

- these nodes are called multi actors, that allow us to create agents, so its a sum of actors that create multi agents

- it is good cause in this way we create a stateful application, with memory persistence 

- we can create:

### nodes 

actions that the agent will handle

### edges

connection between the nodes

### conditional edges

function that see the state and see if it goes to the next node or to the end/or another node

example:

```python
def route(state):
    if state["needs_tool"]:
        return "tool_node"
    elif state["needs_review"]:
        return "review_node"
    else:
        return END
```


- these names "needs_tool" WE DEFINE AT THE CODE

### state

memory that will persist between the nodes

### tools

tools that the agent can decide if it is necessary to call 

- it enables for example to execute human in the loop before it goes to another step

- it has a different sdk than langchain, but it can also use some functions from langchain package

## why is different from langchain

- it is a chain but it allows LOOPS flow, so it can return to a tool/nodes (you set with edges) if it needs again
