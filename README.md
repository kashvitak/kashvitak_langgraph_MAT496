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
