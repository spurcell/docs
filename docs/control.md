## Control Structures

#### Conditionals

Lo `if` statements are like C apart from not requiring parentheses in the predicate.

```

if x < 1 {
	// do something
}
else if x > 1 {
	// do something else
}
else {
	// x is 1
}

```

And `switch` statements are like C apart from the fall-through behavior – Lo cases do not fall through.

```

switch x {

    case "A":
        // handle case A
        
    case "B":
        // handle case B
    
    // optional
    default:
        // handle the default case
}

```

#### Iteration 

Indefinite iteration is supported with `while` and again, no parens are required in the predicate.

```

while x < 10 {

    // this block executes 10 times
}

```

Iteration over the items in a collection is supported with the `scan` statement.

```

scan items -> (item) {

    // this block executes once per element of items
    // items can be an array, set, or map
}

```

#### Invocation

Because Lo procedures always explicitly signal success or failure, *invocation is a branching construct*, like an if/else statement. Every invocation statement can include a success handler, a failure handler, or both; like an if/else statement, only one branch will be executed.

A handler is a parameterized block of statements that can include some context-specific statements; a handler is not a procedure, though it resembles one.

Below is the full form of an invocation statement. We're calling the procedure referenced by `foo` with arguments `x, y` and on success expecting a binary response that we map onto the parameters `bar, baz`. On failure we're expecting a unary message that we map onto the parameter `err`.

```
foo <- (x, y) -> (bar, baz) {

    // handle success
}
~> (err) {

    // handle failure
}
```

The success handler is always preceded by a straight arrow `->` indicating the success response, whereas the failure handler is preceded by a crooked arrow `~>` indicating a failure response.

If the failure handler is omitted, failure is simply ignored.

```
foo <- (x) -> (bar) {

    // handle success; ignore failure
}
```

Likewise if the success handler is omitted, a success response is ignored.

```
log <- (message) ~> {

    // logging failed!
}
```

If both handlers are omitted, a semicolon is required.

```
foo <- ("hi");
```

In any case, a response to the invocation is required before execution continues onto the next statement, regardless of whether anything is done with it. However, this behavior can be changed with asynchronous invocation.

#### Asynchronous Invocation

Every invocation can be either synchronous (the default) or asynchronous.