# The Lo Model of Distributed Computation

*Lo is a language for specifying the behavior of a network of independent, isolated ("shared-nothing"), services communicating asynchronously. Here's what that means.*

--

A message is sent to an address.

After traveling some distance in space and time, the message is delivered to a queue.

A worker tending the queue removes the message and inspects its address to identify the procedure to be performed for the message. 

The procedure is specified as a sequence of the small set of primitive operations that the worker is capable of performing: data storage and retrieval, arithmetic, iteration, sending messages, creating new services (procedures bound to addresses), or halting.

An individual performance of a procedure by a worker is called a *task*. The worker creates a record (called an *environment*) to keep track of the state of its task, including the contents of the message that initiated it. Only one worker may work on a task at a time, and a worker can only do one thing at a time – so only one operation at a time can take place within an environment.

A worker can trade off working on one task for another, or rapidly switch between several tasks in progress, but the worker maintains no state independent of its tasks, nor can it access any other state.

#### Requests and Responses

Addresses are simply values that can be stored, copied, and sent in messages like any other value. However, addresses can only be created by defining a service, and can never be modified.

Every message necessarily has a sender; if a message includes a *return address*, a *response* can be sent back to its sender. Otherwise, the message is a *one-way* communication.

A response always signals either success or failure, independently of the content of the response. This can be thought of as a message written on green or red paper.

Once a worker has responded to a request, it cannot respond again – each request has at most one response.

#### Nested Procedures

A procedure can specify a *nested procedure*, which directs the worker to procure an unused address and bind it to the specified procedure in the environment of the current task (the *outer environment*). The nested procedure can reference the outer environment, which implies only one operation at a time can happen in *either* the inner or outer tasks. Since the message queue for the inner service is part of the outer task's environment, no messages for the inner service will be picked up and acted upon if the worker is engaged on the outer task.

A procedure can define any number of nested procedures, and nested procedures can define nested procedures of their own. But within the context of any given task, only one thing can ever happen at a time!

#### Subrequests

In general, a worker will complete an outer task before picking up a new task for an inner service, but there is one exception.



#### Invalid Operations

In the course of executing a task, a worker may come across an operation that it cannot perform. For instance, the worker could be told to fetch the value of an array element at an index beyond the bounds of the array. In these situations, called *invalid operations*, the worker immediately halts execution of the procedure, performing no more operations on the task.

If the task is a request, the worker sends a failure response to the sender.

Any responses already received and enqueued for subrequests will be dropped, as will any responses received for subrequests after the task has failed.


| Operation | Invalid Operand |
| --- | --- |
| Integer division | Zero divisor |
| Array element fetch | Index > terminal index |
| Array element store | Index > terminal index + 1 |
| Map element fetch | Nonexistent key |

Table 1. All Invalid Operations in Lo
