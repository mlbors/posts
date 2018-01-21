# A meeting with the Compiler & Cie #

Through this article we are going to have an overview of how compilation works to try to understand better how programming languages work. It is going to be a brief summary, more like an introduction. Let's get into it!

# Introduction #

We sometimes gladly forget that programming languages are firstly understandable by humans and not directly by computers. The instructions we write cannot directly be executed by our machine and they have to be translated to _machine code_, or _binary_. Basically, we speak a certain language and we want to tell our computer to do something specific, but that last one speaks a different language.

Having an idea of how compilation works can be something very helpful when we write code or when we try to debug something.

Again, as we said in earlier, this article is not a complete course and some concepts that are going to be mentioned here are going to be simplified though they would fill a whole book if they were viewed in deep.

## A look at the  CPU ##

In a few words, the _CPU_, or _Central Processing Unit_ or processor, is the brain of the computer. It controls all parts of the computer and performs various calculations or operations with data. 

A processor is designed to perform instructions one after another. Each instruction that has to be executed is stored in some memory. That memory is like a grid of cells and each of them has a unique address.

Instructions are numbers, but they can represent anything. An instruction is assigned has its own unique numeric code. So, the _CPU_ retrieves values from the memory and decides what to do. Once the processor has completed the action determined by the given code, it requests the following one and repeats the whole process. This sequence of numerical codes that form a program is named _machine code_. Every instruction begins with a number that specifies its nature which is called _opcode_ (operation code).

The operations that the instructions perform are very simple and so we can tell the processor to do some work by writing a sequence of numeric codes. It would nevertheless be a hard task and that is why the _assembly language_ was created. This last one assigns opcodes a name that describes what it does.

After we have written a sequence of instructions using these symbols, we can use a tool named _Assembler_ that will convert these symbols to the appropriate numeric codes that can be executed by the processor.

The processor will also load operands values from the memory after it has loaded the _opcode_. Theses operand will also be stored in the _Assembler_.

Every processor has a certain architecture that describes which simple operations can be performed and which _opcode_ each instruction has. In the end, it means that every processor has its own _assembly language_. So, one program created for one architecture probably won't work on another one it also means that an _Assembler_ written for a specific architecture won't work on another one. That is where we need high-level programming languages and _Compilers_.

The programming language, through specific statements, lets us write down instructions and the _Compiler_ will handle them. In a few words, when we call it, the _Compiler_ will analyse our code, do some operations with it, decide which value will be stored in which memory address and finally generate a code the processor can understand. It means that we don't have to care about the architecture of the _CPU_ because the _Compiler_ will generate the appropriate instructions, if, of course, it exists for the given architecture.

We have to remind ourselves that a _CPU_ has no concept of user interface, graphics, text, operating systems, processes or threads. We can even say that a _CPU_ has no concept of numbers, or even no concept of _binary_. A _CPU_ acts on electrical charge. When we look at ones and zeros, identifying them at "1" or "0", the computer sees them as voltage levels. A "1" is usually a voltage near the system's power supply voltage, while a "0" is a voltage near ground. A _CPU_ is an electrical device that sends out and receives voltages.

It is also common to use _hexadecimal_ to describe locations in memory. _Hexadecimal_ is a way of presenting numbers in a human readable format, but a _CPU_ knows nothing about it.

## Compiler Components ##

Let's now take a closer look at the _Compiler_ itself. In a few words, the _Compiler_ turns a high-level source code into a low-level object code in machine language, which can be understood by the processor. This process is called compilation.

Regardless of the exact number of phases in the compilation process, a _Compiler_ can be divided in three parts: _Front End_, _Middle End_ and _Back End_.

Basically, the _Front End_ verifies syntax and semantics according to a specific source language. It transforms the input program into an _Intermediate Representation_ (_IR_). The _Middle End_ performs optimizations. Finally, the _Back End_ performs more analysis, transformations and optimizations that are specific to the target _CPU_ architecture. 

### Front End ###

The _Front End_ analyses the source code to build an _Intermediate Representation_. The main phases of the _Front End_ include the following ones:

### Line reconstruction ###

It converts the input character sequence to a canonical form, or standard form, ready for the _Parser_.

### Preprocessing ###

The _Preprocessing_ phase occurs before _Syntactic_ or _Semantic Analysis_. It has to be done for programming languages like _C_ where lines beginning with "_#_" are taken as directives.

### Lexical Analysis ###

Also shortened as _Lexer_, it turns source code into a stream of _tokens_. A _token_ is a pair consisting of a _token_ name and an optional _token_ value. The _tokens_ are the "words" of the programming language and common _token_ categories may include identifiers, keywords, separators, operators, literals or comments. A _token_ is essentially a representation of each item in the code at a simple level.

### Syntax Analysis ###

Also known as _Parsing_, this phase parses the _token_ sequence to identify the syntactic structure of the program. So the _tokens_ are turned into an abstract syntax tree, which is a representation of the source code and its meaning.

### Semantic Analyzer ###

It adds semantic information to the _Parse Tree_ and builds the symbol table. It also checks if the sentences make sense. Checks performed are type checking, object binding or definite assignment.

## Middle End ##

The _Middle End_ performs optimizations on the _Intermediate Representation_ in order to improve the performance and the quality of the produced _machine code_. Those optimizations are independent of the _CPU_ architecture that is targeted.

### Analysis ###

It gathers information from the _Intermediate Representation_. This is required for the following step.

### Optimization ###

The _Intermediate Representation_ is transformed into functionally equivalent but faster or smaller forms.

## Back End ##

The _Back End_ is in charge of specific optimizations and for code generation for the _CPU_ architecture that is targeted.

### Machine dependent optimizations ###

Here are made optimizations that depend on the details of the _CPU_ architecture that the compiler targets.

### Code Generation ###

The _Intermediate Representation_ is transformed into machine language.

### Linker ###

The _Linker_ takes the object files generated by the _Compiler_ and combines them into one executable program. The Linker will arrange the pieces of object code so that functions in some pieces can successfully call functions in other pieces.

## Ahead-of-Time and Just-in-Time ##

The terms _Ahead-of-Time_ (_AOT_) and _Just-in-Time_ (_JIT_) refer to when compilation takes place. A _JIT Compiler_ compiles the program as it is running while an _AOT Compiler_ compiles the program before it is run.

_AOT Compilers_ can perform complex and advanced code optimizations. It provides a global overview of the code.

_JIT_ compiles the code when it is needed, but not before runtime. When a program is executed, the compiled object code is invoked instead of interpreting the entire _bytecode_. A _JIT Compiler_ converts the _bytecode_ to a platform specific executable code that can be executed immediately.

## Interpreter ##

Until now, we've only talked about compiled languages. Some languages are qualified as interpreted because the source code is left as it is, or it is compiled into some "universal" assembly code, and when then program needs to be run, an _Interpreter_, that can read the code, will translate it on the fly for the targeted architecture.

The main advantage of this approach is that it improves portability, safety and flexibility. The main downside is that the speed is reduced because more operations before an instruction can be made.

In _Java_, for example, the source code is transformed into an assembly code that is called _bytecode_ that will be run by the _Java Virtual Machine_ (_JVM_). Another example is _PHP_ where, most of the time, the source code is interpreted by the _Zend Engine_.

An _Interpreter_ may be a program that either:

* Executes the source code directly
* Translates source code into some efficient _Intermediate Representation_ and immediately executes it
* Explicitly executes stored precompiled code made by a _Compiler_ which is part of the _Interpreter_ System

An _Interpreter_ executes a program and may or may not have something like a "_JITter_".

An _Interpreter_ and _Compiler_ can be combined into a single language execution engine. We can combine an _AOT Compiler_ with an _Interpreter_. It can be useful when we have a very high-level language: this language will be first compiled into a language that is easier to parse and easier to interpret. A _JIT Compiler_ can also be combined with an _Interpreter_. Here, we don't have multiple stages like the previous case, but two actors working side-by-side on the same language. So, while the _JIT Compiler_ is busy compiling the code, the _Interpreter_ can already start running the code. Once the _JIT Compiler_ has finished its work, we can switch execution over to the compiled code.

## Conclusion ##

Through this article we took a little look at the Compilation process. Again, this was only a brief overview. Nevertheless, we saw how the _CPU_ works with our code and what needs to be done to it before the _CPU_ handle it. We also looked at the main components of the _Compiler_ to get an idea of what each one does. We also talked about the fact that the compilation can happen at different times, _Ahead-of-Time_ (_AOT_) or _Just-in-Time_ (_JIT_).

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!