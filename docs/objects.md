# Objects

Visibility is *dynamic* and is captured in the topology.

Visibility is a weak form of access control.

A `service` statement creates a service having the specified procedure but is also an expression with a value of the address of the resulting service, which can then be held and transferred like any other value - i.e. assigned to a variable or data structure element, or passed in a message. In this way a procedure can be defined in one module but invoked by another - and thus provide mediated external access to the state of its parent procedure - and thus become a stateful service. This provides the mechanism by which we construct objects in Lo:

```
// a Person constructor

receive name, birthday;

reply {

    getName: service:
	    reply name;
	    
	getBirthday: service:
	    reply birthday;
};
```

Though possible in the language, it obviously doesn't make sense to create a service without retaining its address - this would be doing the work of setting up a new inbox and then throwing away its address.
