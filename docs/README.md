# The Lo Language Reference

> Programmers are always surrounded by complexity; we cannot avoid it. Our applications are complex because we are ambitious to use our computers in ever more sophisticated ways. Programming is complex because of the large number of conflicting objectives for each of our programming projects. If our basic tool, the language in which we design and code our programs, is also complicated, the language itself becomes part of the problem rather than part of its solution.
> 
> *Tony Hoare, 1980 ACM Turing Award Lecture*

A Lo program consists of a **main module** containing a `main` procedure to which a root request is sent to activate the program, and any modules referenced directly or indirectly by the main module.

A **module** is a series of constant definitions, usually procedure definitions, that can be referenced by other modules.

A **procedure** is a series of statements, with optional parameters.

Lo shares its basic syntax with C-family languages so it should feel familiar to most users: simple statements are separated by semicolons and compound statements are enclosed in braces.

However, every construct in Lo is either a statement or an expression; no construct is both.