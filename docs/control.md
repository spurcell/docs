## Control Structures

#### Conditionals

Lo `if` statements are like C apart from not requiring parens in the predicate.

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

And `switch` statements are like C apart from the fall-through behavior

```

switch x {

}

```

#### Iteration 

Indefinite iteration is supported with `while` and again, no parens are required in the predicate.

```

while x < 10 {

}

```

Definite iteration is supported with the `scan` statement.

```

scan items -> (item) {

	log(item);
}

```