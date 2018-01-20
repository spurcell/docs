Every program – like every worker – needs two things to do its work: *skill* and *authority*. A program’s skill is the content of the program itself, though some programs are constructed in a way that makes them seem to acquire new skills during execution. A program’s authority is its ability to access or modify data beyond its input arguments and can only be obtained from its environment of execution.

For example, a program to analyze an input string and produce a set of statistics such as the number of words, sentences, and paragraphs in the string requires zero authority and is an exercise of pure skill applied to an input. By contrast, a program that simply copies data from one buffer to another is almost pure authority, requiring the ability to read from the source and write to the destination, with essentially zero skill.

But what about a program that analyzes a network data stream to identify suspicious behavior? This requires a combination of skill and authority, but that program can be decomposed into simpler pieces that look like the earlier examples – a data movement module and a data analysis module. By factoring the program in this way we’ve removed the need for any authority from the data analysis module and thus rendered it untrusted code. This factoring enables us to incorporate different analysis algorithms from untrusted sources, safe in the knowledge that we’re not trusting them with access to the network streams themselves.

In a secure system, no amount of skill can overcome a lack of authority.





Computer is a misleading holdover from the time when all computers did was compute. Computation is a very bounded concept involving transforming inputs to outputs according to deterministic rules (algorithms). It says nothing about moving data from one place to another, or consuming entropy, or dealing with failure. So right off the bat we’re misled.

What computers really do is work, that is, information processing.

Lo enforces a chain of trust for delegating authority by doing away with global variables and.

Lo modules are purely skill. Access to a module cannot directly confer authority – unless the module contains information that can grant authority via another system, which is an anti-pattern equivalent to storing credentials in source code.

All authority available to a Lo program is injected by its environment into its root service at ignition.
