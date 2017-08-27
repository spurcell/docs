## Primitive Data Types

Lo's primitive data types include

- integers
- floating-point numbers (IEEE 754)
- boolean values (`true` and `false` are literals)
- characters
- procedure addresses

There are no reference types in Lo apart from procedure addresses; shared data is not possible.

**Variables** are declared implicitly by assignment:

```
myInt = 42;
myFloat = 42.0;
testMode = true;
```
*Note: assignment statements in Lo are not expressions.*

**Constants** are defined using `is`:

```
theAnswer is 42;
planetName is "Tralfamadore";
```

Once a constant has been defined it cannot be redefined in the same scope.

A character value holds a single Unicode code point. Character literals can be specified with single quotes. Characters may consume more than one byte.

```
letterA = 'A';
aleph = 'א';
```

### Expressions

Lo provides the usual non-mutating operators for constructing expressions.

- Algebraic: `+` `-` `*` `/` `%`
- Logical: `and` `or`
- Testing: `==` `>` `<` `>=` `<=`

Lo also provides mutating operators that aren't allowed in expressions.

- Increment/decrement: `++` `--`
- Assignment: `=` `+=` `-=` `*=` `/=` `%=`

Function application expressions of the form `foo(x)` are syntactic sugar for a special case of procedure invocation; this is described in detail under [Control Structures](control.md).

## Data Structures

Data structures in Lo are passive chunks of data that can be operated on, like arrays in C, not full-blown "objects" in the OOP sense. They also can't be shared; like primitives, data structures are strictly local objects that can only be referenced by an enclosing procedure. So if you "pass" a data structure as an argument to a procedure, its contents be copied into the invocation message, and the local copy can't be seen or modified by the invoked procedure.

Constants can be defined as data structures which are then immutable.

### Compounds

A **compound** is a data structure comprising two or more **components** of any data type – including collections or other compounds – each of which has a distinct name. Components are specified in expressions using the dot operator `.`

Compound literals are defined within parentheses, with a colon separating component names from their values.

```
student = (
	name: (first: "Joe", last: "B")
	course: 16
	year: 2001
);

// specify components by name using the dot operator
fullName = "`student.name.first` `student.name.last`";
```

(Since the convention appears to be that every language invents its own term for this concept, Lo adheres to this convention. See: record, struct, tuple, object.)

### Arrays

An **array** is a sequence of any number of elements, which can be thought of as a strip of paper divided into cells. Arrays grow and shrink dynamically as elements are added or removed; an array can't have "missing" elements. Elements can be addressed (in constant time) using the zero-based subscript operator `[]`. Array literals can be defined within square brackets.

```
fibs = [0, 1, 1, 2, 3, 5, 8];
articles = ["a", "an", "the"];
mixed = [0, 1, 2, "abc", [4, 4]];
empty = [];

fibs[3];		// 2
fibs[fibs[3]];	// 1
fibs[-1];		// syntactic sugar for terminal element (8)
fibs[-2];		// syntactic sugar for 2nd-to-last element
```

The range notation `..` specifies the portion of an array from a starting index *up to and including* an ending index, which can be omitted to indicate "to the end of the array".

```
fibs[1..4];     // [1, 1, 2, 3]
fibs[4..];		// [3, 5, 8]
```


A **string** is simply an array of characters; literals are defined between double quotes.

```
name = "Pirate Prentice";

```
Expressions can be interpolated into string literals using backticks.

```
message = "Your circle has area `PI * r * r`";
```

and converted to strings with bare backticks.

```
write(`height`);
```

The concatenation operator `><` evaluates to a new array containing the elements of the left operand followed by the elements of the right operand. It doesn't modify either of its operand arrays.

```
western is ["K2", "Nanga Parbat"];
eastern is ["Everest", "Lhotse", "Kangchenjunga"];

// evaluates to the concatenation of the two immutable arrays
mountains = western >< eastern;        
```


### Sets and Maps

A **set** is an unordered collection of same-type values. Set literals are defined with braces, with commas as optional delimiters.

```
crew = {"Leela", "Fry", "Bender"};
```

A **map** is a set of keys, each with an associated value. A map thus defines a mapping from the set of keys onto the set of values. Since a map is an extension of the set concept, map literals are also defined within braces, using the yield symbol `=>` to separate keys and values, with commas as optional delimiters. Map values are referenced in expressions by using the key value in square brackets.

```
greats = {
	"Benny Goodman"      => "Clarinet"
	"Fats Waller"        => "Piano"
	"Fletcher Henderson" => "Clarinet"
	"Jelly Roll Morton"  => "Piano"
};

// reference a value with a literal key
instrument = greats["Benny Goodman"]; // Clarinet

// add a new key
greats["Louis Armstrong"] = "Trumpet, Vocals";

// reference a value with an expression
first = "Fats";
last = "Waller"
instument = greats["`first` `last`"];

emptyMap = {=>};    // to distinguish from an empty set
```

#### Set Operations

You can get the cardinality (number of elements) of a collection (array, set, or map) using the count operator `#`. Since a record is semantically a single object, not a collection, it doesn't have a cardinality.

```
numItems = #{"apples", "oranges"};    // numItems is 2

// since a string is an array, we can get its cardinality
strlen = #"monkeys";                  // strlen is 7
```

You can test membership in sets and maps using the `has` operator:

```
if crew has "Bender" {
	// keep an eye on your belongings
}

if greats has player {
	instrument = greats[player];
}
```

You can find the union, intersection, or difference of two sets or maps using the `+`, `*`, and `-` operators, respectively. These operators provide the ability to insert and remove elements from sets.

```
humans  = {"Amy", "Professor", "Hermes", "Fry", "Leela", "Scruffy"};
aliens  = {"Zoidberg", "Kif", "Nibbler"};
robots  = {"Bender", "Bessie"};

// union
crew = {"Leela", "Fry"} + robots;

// intersection
humanCrew = crew * humans;

// difference
nonHumanCrew = crew - humans;

// insert a set element
humans += 'Zapp';

// remove a set element
robots -= 'Bessie';
```

Union of maps is non-commutative: it will contain the left operand's values wherever both maps define the same keys. 
Intersecting a set and a map is also non-commutative: the result is a set if the set is the left operand; otherwise a map.
