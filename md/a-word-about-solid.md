# A word about SOLID #

Through this article, we are going to take a look at the principles hidden behind the _SOLID_ acronym.

## Introduction ##

In computer programming, _SOLID_ is an acronym for five design principles. They applied to object-oriented programming, but, in a way, can be extended to programming in general. These principles were introduced by _Robert C. Martin_, also known as _Uncle Bob_, in the early 2000s.

These principles are not laws, but they can help us write better code. Applying those principles allows us to have a low coupling, high cohesion and strong encapsulation. Our code will be easy to maintain, to reuse or to extend. In other words, we can achieve scalability and avoid our code to breaks after every single change.

## Principles ##

As we said, _SOLID_ is an acronym and every letter stands for the following principles:

* **S** – Single-responsibility principle
* **O** – Open-closed principle
* **L** – Liskov substitution principle
* **I** – Interface segregation principle
* **D** – Dependency Inversion Principle

Let's have an overview of these different principles.

## Single-responsibility principle ##

> "_There should never be more than one reason for a class to change_"

The _Single Responsibility Principle_ (_SRP_) asserts that a class or module should do one thing only. This principle states that an object should only have one responsibility, in other words, a reason to change, and it should be completely encapsulated by the class. In other words, we have to avoid our class to be a _Swiss Army Knife_!

We have to ask ourselves if the logic we are implementing should live in the class we are writing. It is a good practice to split big classes in smaller ones and to avoid "_god classes_".

## Open-closed principle ##

> "_Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification_"

This principle states that any class should be written in a way that it can be used as it is. It can be extended if needed, but it can never be modified.

"_Open to extension_" means that we should create our classes so that new functionality can be added while new requirements are generated. "_Closed for modification_" means that once we have developed a class we should never modify it, except to correct bugs.

If we apply this principle, we will get a loose coupling and it will improve readability. It will also reduce the risk of breaking existing functionality.

## Liskov substitution principle ##

> "_Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program_"

The main idea here is that objects should be replaceable by instances of their subtypes without affecting the functioning of the system.

This principle applies to inheritance hierarchies and it states that classes should be designed in a way that the client dependencies can be substituted with subclasses without the client knowing about that change. All subclasses must operate the same manner as their base classes. The specific functionality of a subclass may be different, but must conform to the expected behaviour of its base class.

So, the parameters in subclasses must either be the same types as those in the base class or must be less restrictive. (contravariance) In the same way, the subclasses return types must be the same as the base class return types (covariance).

To apply this principle, we also need to take care of the preconditions and the postconditions. A precondition of a class is a rule that must be in place before an action can be taken while a postcondition describes the state of objects after a process is completed. The principle says that the preconditions must not be strengthened by a subclass and the postconditions cannot be weakened.

Here, it also says that the conditions of a process that is true before that same process begins and remains true afterwards (invariants) must be preserved. This principle also has a rule called history constraint saying that, in a subclass, new or modified members should not modify the state of an object in a way that would not be permitted by the base class.

There is another constraint saying that no new exceptions should be thrown by methods of the subtype, except where those exceptions are themselves subtypes of exceptions thrown by the methods of the supertype.

## Interface segregation principle ##

> "_Classes that implement interfaces should not be forced to implement methods they do not use_"

In other words, "many client-specific interfaces are better than one general-purpose interface". We don't want to force clients to depend on things they don't actually need. This gives us low coupling and high cohesion.

## Dependency Inversion Principle ##

> "_One should depend upon abstractions, not concretions_"

This principle states that high-level modules should not depend upon low-level modules. By depending on higher-level abstractions, we can easily change one instance with another instance in order to change the behavior.

## STUPID ##

The _SOLID_ acronym could be opposed to the _STUPID_ acronym that describes what we should not do:

* **S** - Singleton: singleton is often considered as an anti-pattern, programs using global state are very difficult to test and hide their dependencies
* **T** - Tight Coupling: tightly coupled modules are hard to reuse or to test
* **U** - Untestability: testing should be easy
* **P** - Premature Optimization: it is sometimes better to keep things simple than to have an unreadable code that brings only costs and no benefits
* **I** - Indescriptive Naming: don't forget that programming languages are for humans
* **D** - Duplication: write code only once, keep it stupidly simple (KISS), and don't repeat yourself (DRY)

## Conclusion ##

Through this article, we briefly saw what _SOLID_ stands for and what principles are hidden behind this acronym. So, again, it can be summarized like so:

* **S** – Single-responsibility principle
* **O** – Open-closed principle
* **L** – Liskov substitution principle
* **I** – Interface segregation principle
* **D** – Dependency Inversion Principle

Following those principles will help us write better code, but we have to remember that they are principles and not absolute laws.

Now, some can argue that the _SOLID_ acronym just stands for marketing purposes and we don't understand _OOP_ better thanks to those principles. Indeed, this acronym can sell well, but keeping those principles in mind, even if they seem pretty obvious or too basic, can't hurt.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!