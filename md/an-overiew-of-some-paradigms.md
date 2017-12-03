# An overview of some Paradigms #

In this article, we are going to have an overview of some programming paradigms to get a little more familiar with them.

## Introduction ##

Here we are not going to look at each programming paradigm that exists, because it would be too complex. We are going to have an overview of some popular and common paradigms. Our aim is to define them with simple terms and try to understand them better.

## What is a paradigm? ##

A paradigm is like a set of concepts or thought patterns that includes theories, research methods or postulates. It is a system of assumptions that serves as a model. It is a way to see the reality.

## What is the relationship with programming? ##

In programming, paradigms are a way to classify programming languages based on their features. Each programming language follows one or more paradigms. Most of the major language paradigms were invented between the late 1960s and the late 1970s.

Some paradigms are concerned with implications for the execution model of the language, which specifies "_how work takes place_". So, they are concerned with such things as allowing side effects, in other words, when a function modifies the state of something else outside its own scope, or whether the sequence of operations is defined by the execution model. Other paradigms are concerned with the way that code is organized or with the style of syntax and grammar.

## Imperative ##

Also known as _Procedural Programming_, _Imperative Programming_ uses statements that change the state of a program. In a way, it is like following a recipe. The _Imperative Programming_ is concerned with defining a linear procedure or sequence of programming statements. It consists of commands for the computer to perform. It could be summarized with the sentence "_first do this, next do that_".

The _Imperative Programming_ approach is to treat any solution like a series of steps to be performed. So, it uses procedures, functions, subroutines or methods to split the program in small tasks and that makes possible to reuse the code as many times as we want in the program.

The first _Imperative Languages_ were the machine languages and the hardware implementation of nearly all computers is _Imperative_ because it is designed to execute machine code.

Representative programming languages that use this paradigm would be _FORTRAN_, _COBOL_ or _BASIC_.

## Declarative ##

In a few words, _Declarative Programming_ describes what it does, but not how it does it. So, we program by specifying the result we want, but not how to get it. It means that the control flow is implicit. It means no loops and no assignments.

Representative languages would be _HTML_, _CSS_ or _SQL_.

## Structured ##

The _Structured Programming_ paradigm removes _global variables_, _GOTO_ and introduces _local variables_. It means that we create blocks that contain instructions that don't depend on any other. Here, the control flow is defined by nested loops, conditionals and subroutines. It also has in main traits things like structograms and indentation.

Representative programming languages that use this paradigm would be _C_ or _Pascal_.

## Functional ##

_Functional Programming_ treats computations as the evaluation of mathematical functions and avoids changing-state and mutable data. So, it sees all subprograms as functions that have arguments and return a single solution based on the input. It means that every time a function is called with the argument _x_ it will produce the same result _f(x)_.

This paradigm allows to pass functions to functions and to return functions from functions. Here, the control flow is expressed by combining function calls, rather than by assigning values to variables.

Representative programming languages would be _Haskell_, _Erlang_, _Scala_, _Scheme_ or _Lisp_.

## Logic ##

The _Logic Programming_ paradigm expresses a set of sentences in a logical form. It means that logical assertions about a specific situation are made, then it is checked if it still true or not. It can be viewed as controlled deduction. We focus on facts stored in memory, called the knowledge base.

It is a form a _Declarative Programming_ and the process flow is reversed because it asserts result first.

One representative language would be _Prolog_.

## Object-Oriented ##

In _Object-Oriented Programming_, or _OOP_, data is represented as objects. These objects are instances of classes that have attributes, representing data fields, and functions called _methods_. Objects are separate entities and have their own state which is modified built in methods. In _Object-Oriented Programming_, programs are based on the sending of messages to objects, that respond by performing operations. This concept also provides _data encapsulation_ and _information hiding_ to protect internal properties of an object.

One representative language would be _Smalltalk_. However, many programming languages are multi-paradigm and support some elements of _OOP_ and combine them with an _Imperative_ style, like _Java_, _C++_, _PHP_ or _Python_.

## Event-Driven ##

Here the control flow is determined by events, such as user actions. It means there are _event emitters_ and _listeners_. It is dominant in graphical user interfaces. Nevertheless, a lot of software relies on user events for functionality, so we could argue that _Event-Driven Programming_ is present for nearly all kinds of projects.

Representative languages would be _JavaScript_, _ActionScript_ or _Visual Basic_.

## Parallel Programming ##

When we talk about _Parallel Programming_, we mean that we use parallel hardware to execute computation more quickly. It is mainly concerned with speeding-up computation time. So, multiple actions are executed at the same time.

_Parallel Programming_ could be achieved with _C++_ and _OpenMP_, for example.

## Concurrent Programming ##

In _Concurrent Programming_ several computations are executed concurrently. So, it means that only one statement is executed at any point in time, but we have no guarantee which task will be executed in the next step. One task doesn't have to wait that another one ends before starting.

_Concurrent Programming_ could be achieved with _Scala_, _Go_, or _Elixir_.

## Multi paradigms ##

Many programming languages combine multiple paradigms to get the advantages of each. This gives us more freedom and flexibility, but it could also increase the complexity of the programs themselves.

_C++_ and _Java_ fit into that category.

## Conclusion ##

Through this article, we saw what a paradigm is and how it is related to programming. We also had a brief overview of some major programming paradigms by looking at their main concept and how they apply it. We also saw that some programming languages are multi paradigms to give more freedom and to write programs that suit the nature of a specific problem. Many more paradigms exist and some depend on some major paradigms we saw.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!