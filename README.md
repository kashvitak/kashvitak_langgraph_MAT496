# kashvitak_langgraph_MAT496
Large Language Models

LANGGRAPH

MODULE 1:

notebook 1-
simple_Graph:

I tells us the baiscs about graphs, it shows us how to implement the simplest graph, which has a normal edge and a conditional edge.
it tells us about state and node.
state is basically a dictionary with one key which is the graph state
and, node is what takes the value of the state and overrites the value of the graph state.
the normal edge is an edge that is implemented in every possibilites, but a conditional edge is implemented according to the probabilities and the condiditions specified.

notebook 2-
chains:

it teaches us to use messages as a graph state;
we use tools for the model to give us an output,
we define a tool and the model looks for a tool the fulfils the required input schema and then it gives the output based on the schema.
we also used reducer function here, which is used when instead of ovverwriting the graph state, our node will just append the graph state so that it preserves the whole convo.


notebook3-
router:

router is basically a graph, which has a conditional edge;
it either takes the node which used a tool that we have defined to respond to the human messgae if the message is in schema of the tool;
or, it used natural language to answer to the human message as the second node;


notebook4-
agent:
we build an agent that helps us make sequential tool calls;
in the graph, we use conditional edge and gives us 2 options for nodes;
when we give a message out to the model, it can decide whether the question has an input schema related to the call, if it does, it goes to the tool node and then in a loop back to the model, to check if the question uses any more tools and if not it goes to the other node which is end.

we also saw in the video how we can see our traces in langsmith of every input we gave to the model.

notebook5- 
agent memory:

it uses thread in which checkpoints are associated together;
we put in a message and the tool is activated, it gives an answer, if we ask a question using reference to that answer, the model will simply give either incorrect answer or say something like question incomplete.
But, when we use threads, the answer given earlier by the model in the graph is saved in the memeory as a checkpoint whose refernce can be used later in another question to get a correct answer.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

MODULE 2:

notebook1-
schema is the sturcture and types of datatype the graph will use;
state is passed into stategraph;
there are 3 major types to establish schema for your graph that are defined in this nb-
1. typedict
2. python's dataclasses
3. pydantic
the problem with typedict and dataclass is that type hints are not actually enforced at runtime
so it is possible to assign an invalid value without raising an error

here, pydantic comes in;
it is used to provide data validation.
