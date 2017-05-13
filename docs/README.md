
> Programmers are always surrounded by complexity; we cannot avoid it. Our applications are complex because we are ambitious to use our computers in ever more sophisticated ways. Programming is complex because of the large number of conflicting objectives for each of our programming projects. If our basic tool, the language in which we design and code our programs, is also complicated, the language itself becomes part of the problem rather than part of its solution.
> 
> *Tony Hoare, 1980 ACM Turing Award Lecture*

## The Lo Model of Computation

A message is sent to an address.

After traveling nanometers or light-years, the message is delivered to a queue.

A worker tending the queue removes the message and inspects its address, identifying a procedure bound to the address. The procedure is specified as a sequence of the few primitive operations that the worker is capable of performing: data storage and retrieval, arithmetic, iteration, sending messages, and creating new services (procedures bound to addresses).

An individual performance of a procedure by a worker is called a *task*. The worker creates a record (called an *environment*) to keep track of the state of its task, including the contents of the message that initiated it. Only one worker may work on a task at a time, and a worker can only do one thing at a time, so only one operation at a time can take place within an environment.

A worker can trade off working on one task for another, or switch between many tasks in progress, but the worker maintains no state independent of its tasks, nor can it access any other state.

A procedure can specify a nested procedure, which tells the worker to procure an unused address and bind it to the specified procedure in the environment of the current task (the outer environment). The nested procedure can reference the outer environment, and will therefore be performed by the same worker as the outer task, which means only one operation at a time can happen in either the inner or outer tasks. The message queue for the inner service is part of the outer task's environment, so no messages for the inner service will be acted upon if the worker is engaged on the outer task.

A procedure can define an unlimited number of nested procedures, and nested procedures can define nested procedures of their own. But within the context of any given task, only one thing can happen at a time.

Addresses are simply values that can be stored, copied, and sent in messages like any other value. However, addresses can only be created by defining a service, and can never be modified.

In general, a worker will complete an outer task before picking up a new task for an inner service, but there is one exception.