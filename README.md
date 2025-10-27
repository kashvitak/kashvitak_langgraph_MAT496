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

notebook 2-
state reducers

reducers- define how updates are performed on specific keys or channels in our schema
when we make updates, we override the prior value
simulataneous updates in the same key in the same step is ambiguous because you are overriding th value with 2 different nodes at the same time;
reducers allow us to specify how to perform state updates
allows us to append rather than override
the add_messages reducer allows us to append messages to the messages key in our state
if we give an id to a msg same as an existing msg, the msgwill get overriden

notebook3-
multiple schemas

(state: state1) -> state2:
(this typehint means taking in state 1 but writing out to state 2)
whatever is not in the overallstate will not be included in the output

we also used typed input and output schemas

notebook4-
trim-filter-messages

A simple way to use prebuilt add_message reducer that we have built in messagestate along with removemessage to delete messages by id from message queue

we can just ask the llm to graph a particular message from the message list

during long messages, there is high token usage so we can use reducers;

notebook 5-
chatbot summarization

creating a key called summary in the messages_state (not using the built in message key)
summary contains the summary of the conversation

we have an assistant that has the ability to summarize once the conversation gets too long( after 6 messages in this case)
and the running summary lives in state

we use a checkpointer to persist state through time so we can keep having this conversation with this assistant over time
and we are gonna produce running summary so that this conversation never gets too long to exceed token limit

when we take a look at the tracing, we see that as we pass an input, we also get the previous conversation along with that which has been stored in the state as a checkpointer;
as soon as we reach to 6 messages in this case, we can see that a summary is produced in a kind of continuously updated summarization state

notebook 6-
SQLite is a popular SQL database 
and used for working with external database checkpointers
benefit of SQLite checkpointer- we are writing to a local databse on a machine, so its persisted over time

if, we were to restart the kernel, the memory will be removed from the history if we would have been using a internal checkpointer,
but since we are using SQLite which is an external checkpointer, the state will be locally saved in the machine

when we go to studio in this, we can see that we have local persistence through postgress in this particular case because the langgraph API is packaging our code with a persistence layer automatically that is serving the studio here and it can manage all our threads through studio

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
MODULE 3
-----Notebook1- streaming-interruption----

There are 2 ways to stream the state,
one is updates where we just stream the updates to state after each node is called
the other is values, which is the full state after each node is called.

if event is metadata, we are printing that out
if its is messages/partial, we do processing i.e we get the data from that

we are streaming the output or messages from the chat model whether they're just tool calls or whether they're just natural language processing.

------Notebook2-breakpoints------

how breakpoints work-
u can stop a graph at any node,
then u can proceed just by passing none with a thread id and it will pick back up from the current state of the graph

we can pass interrupt_brefore to our agent through the API

using the API we can do two things-
one, we can pass the interrupts via just the arguments
or, we can define it in the code.

--------Notebook3- edit-state-human-feedback----

Apart from approval to proceed, breakpoints also provide us with an opportunity to actually modify the graph state because the graph state is stopped and we can manipulate and grab the state as we want.

we can apply state update
and then choose to look at the new state of the graph

we interrupt before assistant in this module,
so we run the message and we only get the human message as we are interrupted before the assistant where we can modify our state if we want and then we proceed and apply tool call if so and go back to the assistant node and stop again as it is interrupted again and we have to give it permission again.

in studio, we can manually add interrupt at any point in the graph

we can also get user input to modify the state
by using dummy node also called as no-op node taht will accept the user feedback and inject it into our graph at a particular point.

--------Notebook4- dynamic-breakpoints----------

we can have the graph to be able to interrupt itself- internal breakpoint
that breakpoint will be based on some condition

the above can be done by using NodeInterrupt

we can bypass our NodeInterrupt by updating the state and then continuing to the graph;
which means, an interruption will be there due to not following some condition and to bypass that state or to move on from it, we will need our input to follow that condition, so we will update our input to follow that condition so that our graph can move forward.
