## Control Structures

#### Conditional Statements

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

// single statement form
if answer == 42 log <- answer;

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

Indefinite iteration is supported with `while` and again, no parentheses are required in the predicate.

```
while x < 10 {

    // this block executes 10 times
}

```

Iteration over the items in a collection is supported with the `scan` statement, which executes a parameterized block for every element of a collection. Array elements are processed sequentially; the sequencing of elements in a set or map is arbitrary but consistent.

```
scan items -> (item) {

    // this block executes once per element of items
    // items can be an array, set, or map
}

```

#### Invocation

Because Lo procedures always explicitly signal success or failure, *invocation is a branching construct*, like an if/else statement. Every invocation statement can include a success handler, a failure handler, or both; like an if/else statement, only one branch will be executed.

A handler is a parameterized block of statements that can include some context-specific statements; although it resembles one, a handler is not a procedure.

Below is the "full form" of an invocation statement. We're calling the procedure referenced by `foo` with arguments `x, y` and on success expecting a binary response that we map onto the parameters `bar, baz`. On failure we're expecting a unary message that we map onto the parameter `err`.

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

In any of these cases, a response to an invocation must be received before execution continues onto the next statement, regardless of whether anything is done with it. However, this constraint can be removed by employing asynchronous invocation.

#### Asynchronous Invocation

Any invocation in Lo can be made asynchronous simply by preceding the invocation statement with the `async` keyword or its shorthand form, `@`. 

```
async getSession <- userID -> (session) {

    // handler runs only after all following statements have completed
}
~> (err) {

    log <- err;
}

// following statements
```

With asynchronous invocation, execution *does not wait* for a response to the invocation; the next statement is executed immediately. Asynchronous logic can be confusing because execution becomes non-linear, but *following statements will always execute before any async invocation handlers are run*.

When a response is received for the async invocation it gets enqueued and will be processed only after the procedure has completed. If the procedure completes before all responses have been received, the procedure will hang until all responses are received. Thus every procedure execution in Lo has two phases: running, and waiting for responses.

#### Function Application Syntax

Function application is a semantic primitive in most languages, but in Lo, expressions of the form `foo(x, y)` are syntactic sugar for a special case of procedure invocation.

A function application denoted as e.g. ƒ(x) is a mathematical concept meaning "the value of computing the function ƒ with the value x". However, a computer program isn't math; it's machinery. Lo's concept of procedure invocation fits into function-application semantics provided *the procedure always replies with a unary success response*.

- If the procedure replies with a non-unary response, the entire response is shoehorned into the value of the function application expression, as a compound data object.

- If the procedure replies with a failure, the value of the function application expression is unknown, and the program has no alternative but to halt the current task.

Some languages, such as FORTRAN and Pascal, have distinct concepts of functions and procedures (subroutines); Lo has one general concept (procedure), but two distinct styles of invocation.

Function application syntax is compatible with async invocation syntax:

```
foo(@bar(), @baz());
```

This example invokes bar and baz *concurrently*; once they have *both* responded, foo is called synchronously.