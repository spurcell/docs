# Concurrency

Scopes and concurrency.

An async request creates a new thread of control that is *sequential* with any other threads in scope.

You can reply before the work of a thread is done:

```
main is <-> {

	@foo() -> {
	
		// do stuff *after* our reply is sent
	};
	
	reply 42;
};
```

Default replies happen after all the work associated with a service is complete.
