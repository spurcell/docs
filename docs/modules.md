## Program Structures

A Lo program consists of one or more **modules**, each containing one or more **procedure** definitions, each consisting of zero or more **statements**. (Objects are a *pattern*: a common way of deploying these concepts.)

A **procedure** is a series of statements, with optional parameters. There are two classes of procedure in Lo: **service** and **sink**.

A **module** is a series of constant definitions, especially service definitions, that can be referenced by other modules.

A **program** consists of a **main module** containing a `main` service to which a **root request** is sent to activate the program, and any modules referenced directly or indirectly by the main module.

### Modules

Lo modules are an integral part of the language, not left preprocessor concern. A Lo program is properly understood as a graph of modules (cycles are permitted). A Lo module is simply a file that contains one or more constant definition statements, and optional references to other modules:

```
// a minimal main module
main is <-> (args, io) {
    io.out.write("Hello, world!");
};
```

Note that all Lo modules are *stateless*.

A module can refer to another module using a *module reference*, which associates a name with a *module identifier*. A module identifier uniquely identifies a module to the host system, which is responsible for locating the module and preparing it for use. The associated name is private to the referencing module and is bound to a constant record with a field for each constant declared in the referenced module. The local name is not included in the record for the module that defines it.

Here's a main module that references a local module (specified using just a file name, sans extension):

```
attach "Greeter.lo" as Greeter;

name is "Bigfoot Bjornsen";

main is <-> (args, io) {
    Greeter:sayHi(io.out, name);
};
```

Greeter.lo:

```
sayHi is <-> (writer, name) {
	writer("Hello, `name`!");
};
```

Module references must come before any statements in a module.

#### Dynamic Loading

Sometimes we may want to load a module dynamically; that is, we don't know at compile time which module we'll want to load, we'll only know at runtime. In this case, there's a special procedure available.

### Procedures

A **nested procedure** is always defined by creating a service using the `service` keyword.

```
receive name, x, y;

getLuckyNumber = service:
    receive scalar;
	reply scalar * (x + y);
	
reply "Hello, `name` - your lucky number is `getLuckyNumber(42)`;
```

Note how a nested procedure can refer to its own task's state *as well as the state of any parent tasks*. However, it can't refer to any other state - it is totally cut off from the world otherwise. Therefore, anything required by a procedure must be provided in a request or in the parent request.