# Language Basics

Lo employs a concise, C-family syntax that should feel familiar to most users. Statements are separated by semicolons and braces are used to delimit blocks.


## Data Types

Lo supports signed integers and IEEE-794 floating point numbers and provides the literal values `true` and `false` for booleans.

```
myInt = 42;
myFloat = 2.71828;
```
*Note: assignments in Lo are statements, not expressions.*

Lo provides the usual operators for:

- Algebraic expressions: `+` `-` `*` `/` `%`
- Logical expressions: `and` `or`
- Comparison: `==` `>` `<` `>=` `<=`
- Increment/decrement: `++` `--`
- Modify `+=` `-=` `*=` `/=`

*Note: since the increment and decrement operators are just syntactic sugar for assignments of the form `x = x +/- 1` and assignments are statements, not expressions, these operators cannot be used in expressions.*

A **character** value holds a single Unicode code point. Character literals can be specified with single quotes. Characters may consume more than one byte.

```
letterA = 'A';
aleph = 'א';
```


## Data Structures

Collections in Lo are not objects with message interfaces but more like C arrays: local values that can be directly accessed and modified by the current procedure. So if you pass a collection to a procedure, it may need to be copied.

An **array** is a sequence of any number of elements, which can be thought of as a strip of paper divided into cells. Arrays grow and shrink dynamically as elements are added or removed, but an array can't have "missing" elements. Elements can be addressed (in constant time) using zero-based subscript notation. Array literals can be defined within square brackets.

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

The range index notation `..` addresses a portion of an array from a starting index *up to and including* an ending index, which can be omitted to indicate "to the end of the array".

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

and converted to strings with bare backticks.**

```
write(`height`);
```

The concatenation operator `><` yields a new array containing the elements of the left array followed by the elements of the right array.

```
western = ["K2", "Nanga Parbat"];
eastern = ["Everest", "Lhotse", "Kangchenjunga"];

mountains = western >< eastern;        // combine two arrays

// Q: why does the statement below add one string element instead of
// six character elements to the back of the array?

mountains ><= "Denali";                // insert a value at the back
"Denali" =>< mountains;                // insert a value at the front

// A: because it's an array of strings, not characters
```


A **record** is a sequence of one or more labeled elements of any type, including collections or other records. Records provide direct access to elements using either dot-label notation or zero-based subscript notation. Record literals can be defined within square brackets.

```
student = [
	name: [first: "Joe", last: "B"]
	course: 16
	year: 2001
];

// access elements by label
fullName = "`student.name.first` `student.name.last`";

// access element by position
course = student[1];
```


A **set** is an unordered collection of same-type values. Set literals are defined with braces.

```
crew = {"Leela", "Fry", "Bender"};
```

A **map** is a set of *keys*, each with an associated value. A map thus defines a mapping from the set of keys onto the set of values. Map literals are defined with braces, being an extension of the set concept.

```
greats = {
	"Benny Goodman"      => "Clarinet"
	"Fats Waller"        => "Piano"
	"Fletcher Henderson" => "Clarinet"
	"Jelly Roll Morton"  => "Piano"
};

// access a value
instrument = greats["Benny Goodman"]; // Clarinet

// add a new key
greats["Louis Armstrong"] = "Trumpet, Vocals";

// remove a key, returning the value
cut greats["Jelly Roll Morton"];

emptyMap = {=>};    // to distinguish from an empty set
```

The union operator `+` can be used to merge two sets, maps, or records. If used to merge maps, it is non-commutative; that is, the left-hand-side map keys also present in the right-hand-side map will appear in the union result with the values of the right-hand-side keys.

The intersection operator `*` can be used to intersect two sets, maps, or records.

#### Cardinality

You can get the cardinality (number of elements) of any array, set, or map with the cardinality operator `#`. A record is a composite data type, not a collection, so it doesn't have a cardinality. 

```
numItems = #{"apples", "oranges"};    // numItems is 2
strlen = #"monkeys";                  // strlen is 7
```

