# Standard Modules

The Lo Standard Modules are those in the `Lo` namespace.

## Lo::String

This module contains procedures for working with strings, which are simply arrays of Unicode codepoints.

#### split (string, delimiter) -> string[]

Splits the given string at each instance of delimiter, returning the resulting substrings in an array and discarding all instances of the delimiter.

#### parseInt (string) -> int

Parses a string into an integer.

#### parseInt (string) -> float

Parses a string into a floating point number.