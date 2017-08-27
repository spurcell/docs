### Modules

Lo modules are an integral part of the language semantics, not merely a concatenation. A Lo program is properly understood as a graph of modules (cycles are permitted). A Lo module is simply a file that contains one or more constant definition statements, and optional references to other modules:

```
// a minimal main module
main is (args, system) {
    system.out.writeln("Hello, world!");
};
```

Note that all Lo modules are *stateless*.

A module can refer to another module using a *module reference*, which associates a name with a *module identifier*. A module identifier uniquely identifies a module to the host system, which is responsible for locating the module and preparing it for use. The associated name is private to the referencing module and is bound to a constant record with a field for each constant declared in the referenced module. The local name is not included in the record for the module that defines it.

Here's a main module that references a local module (specified using just a file name, sans extension):

```
Greeter is module "Greeter.lo;

name is "Bigfoot Bjornsen";

main is <-> (args, system) {
    Greeter.sayHi(system.out, name);
};
```

Greeter.lo:

```
sayHi is <-> (writer, name) {
	writer("Hello, `name`!");
};
```

Module references must come before any statements in a module.
