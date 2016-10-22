# Introduction

> Programmers are always surrounded by complexity; we cannot avoid it. Our applications are complex because we are ambitious to use our computers in ever more sophisticated ways. Programming is complex because of the large number of conflicting objectives for each of our programming projects. If our basic tool, the language in which we design and code our programs, is also complicated, the language itself becomes part of the problem rather than part of its solution.
> 
> *Tony Hoare, 1980 ACM Turing Award Lecture*

Why was Lo created? There are already so many "general purpose" programming languages – surely we have enough. The purpose of Lo is to be a tool that feels better in the hand; that doesn't hurt to hold for a long time; that doesn't get in the way of precision work, but can also be used to build large-scale structures. Lo is a tool, not a manifesto. Simplicity is pursued because that makes a better tool. The purpose is developer happiness.

## Requirements

##### Simplicity

See Hoare quote above.

##### Security

The language should be secure by default, and permit development of secure software. That is, it should not have features that violate secure software design. Security *must* be embraced at the language level.

##### Testability

The design of a language is all about defining the border between what the programmer can draw upon vs what he must articulate. The language should not be an obstacle to developing the high-veracity tests required of critical software.

##### Distributibility

Not only should a piece of software not have to be rewritten to be distributed, or recompiled, it shouldn't even have to be relaunched; a program should be able to span compute elements *dynamically*.

## Design Principles

##### A simple, intuitive, and precisely defined conceptual model

Syntax is the mechanism of projection of concepts into text space; the conceptual model *is* the language. The model keeps inconsistencies out of the language. Our facility in programming is a function of how well we understand our virtual machine.

##### "Preservation of Information"

> The language should allow the representation of information that the user might know and the compiler might need.
> 
> *Bruce J. MacLennan, Principles of Programming Languages*

Lo's async control structures are an example of this in practice.

##### Leverage

This is typically called "power" but that term reliably triggers a vapid discussion of the theoretical equivalence of all Turing-complete languages, so I'm going to attempt to sidestep that by referring to the ability of the language to offer commonly-reached-for aspects such as collections as primitives to streamline the developer's. Much of the difficulty of language design lies in defining this border. The higher the leverage, the lower the tedium.

##### Readability

Reading should be an effortless pleasure. Punctuation should be deployed where it enhances readability. E.g. braces and semicolons are used as explicit delimiters of blocks and statements because, perhaps counterintuitively, they let us to read more quickly than not having them.

*Later, graceful degradation could come into play.