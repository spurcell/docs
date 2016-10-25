## Program Structures

The levels of structure in Lo are statements, procedures, modules, and programs. Objects are a *pattern* built using these concepts.

A **module** is a stand-alone procedure, meaning it has no parent procedure.

A **program** consists of a **root module** to which a **root request** is sent to invoke the program, and any modules reachable by an `acquire` sequence leading from the root module (explained below).

### Modules

A module always occupies its own file and is simply a list of statements:

```
// say hello

receive args, io;
io.stdout.write("Hello, `name`!");
```
The `receive` statement maps the request body onto one or more parameter names.

A **nested procedure** is always defined by creating a service using the `service` keyword.

```
receive name, x, y;

getLuckyNumber = service:
    receive scalar;
	reply scalar * (x + y);
	
reply "Hello, `name` - your lucky number is `getLuckyNumber(42)`;
```

Note how a nested procedure can refer to its own task's state *as well as the state of any parent tasks*. However, it can't refer to any other state - it is totally cut off from the world otherwise. Therefore, anything required by a procedure must be provided in a request or in the parent request.
