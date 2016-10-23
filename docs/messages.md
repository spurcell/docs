# Messages and Procedures

Here's some Lo code that probably looks suspiciously familiar to you:

```
y = foo(x);
```

That looks exactly like a typical C-family function call and assignment. And in terms of effect, that's an accurate impression. But the underlying semantics are very different.

Messages are fundamental in Lo; every procedure activation begins with a message. Procedures are defined with the arrow operator (to suggest the incoming message). For example:

```
foo is -> (a, b) {

	// do something here
};
```

Every message has a body (which may be empty) and is sent to an address that is (usually) bound to a procedure.

This is how you send a message with the body "hello" to the address bound to `foo`.

```
"hello" >> foo;
```
Since foo is just a number, we can send it in a message, for instance:

```
("hello", foo) >> bar;
```

Now bar can send a message to foo!

# Services and Requests

What if a procedure wanted to respond to the sender of message to provide a result? The procedure could take the address of a "reply-handling" procedure as an argument.

```
myService is -> (arg, replyTo) {

	// do something
	
	// send a reply
	"hunky dory" >> replyTo;
};
```

Then you would call the procedure like so:

```
("hi there", replyHandler) >> myService;

replyHandler is -> (result) {

	// execution continues in here
};
```

After the message is sent to myService, execution continues inside the handler. But what if our procedure might not always succeed? We need some way to indicate failure. One way to do this would be to take a second address – a "failure-handling" procedure:

```
myService is -> (arg, replyTo, failTo) {

	// do something
	
	// oops, something went wrong
	"darn" >> failTo;
};
```

Now you would call the procedure like so:

```
("hi there", replyHandler, failHandler) >> myService;

replyHandler is -> (result) {

	// execution continues here on success
};

failHandler is -> (result) {

	// execution continues here on failure
};
```

Because this pattern is ubiquitous, Lo provides built-in support for it in the form of *services* and *requests*. A service is defined using the `<->` operator like so:

```
myService is <-> (arg) {

	// do something
	
	if (success) {
		reply "ok";
	}
	
	fail "dang";
};
```

Note that, like our example above, **every service call explicitly succeeds or fails**. In the context of a service procedure, there are two new commands available: `reply` and `fail` that send a message to one or the other channel and halt the execution of the procedure. Note that this is an either-or situation: exactly one of `reply` or `fail` can be executed. Also, exactly one *must* be executed or the requestor will hang. If there's nothing left for a service to do, it sends an empty `reply` to the request.

You call a service with a *request statement*, providing success and failure handlers like so:

```
myService("hello") -> (result) {

	// handle result
}
on fail -> (err) {

	// handle error
}

// execution continues here
```

A request statement is a branch-selection construct similar to an `if` statement if the branch blocks could accept parameters. Once the appropriate handler has completed, execution continues with the next statement after the request, exactly like an `if` statement.

Either handler can be omitted:

```
// ignore failures
myService("hello") -> (result) {

	// handle result
}

// ignore successes
myService("hello") on fail -> (err) {

	// handle error
}
```
When a handler is ommitted, the respective message is still listened for as a signal to continue execution. Imagine if this wasn't true: you only care about handling success, but a request fails – if you weren't listening for that failure you wouldn't know that the success message will never come, and the process would hang. In effect, when no explicit handler is provided, an implicit one is put in its place.

If both handlers are omitted, a semicolon is required:

```
myService("hello");
```

Both handlers being omitted means you don't care about success *or* failure; in this case, execution can continue immediately, and the implicit listening behavior is not necessary.

# Asynchronous Requests

When we introduce the concept of a request we raise the question: what if I want to do something else while I await a reply?

# Request Expressions

The requests we've seen so far have all been statements, but for brevity it's nice to be able to embed requests in expressions.

And now we're back where we began:

```
x = foo(y);
```

Request expressions can also be asynchronous.

Which lets us do this:

```
x = foo(@bar(), @baz());
```

The arguments to foo are evaluated *concurrently*.

# Events

Another messaging pattern is the one-to-many pattern. This has built-in support in the form of *events*.

```
// define an event
```