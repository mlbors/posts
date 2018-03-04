# Covariance & Contravariance #

Through this article, we are going to take a look at some terms like "_Covariance_" or "_Contravariance_" to understand to which concepts they are related to.

## Introduction ##

Sometimes in the happy and sparkling world of programming, we can meet some terms that we have hardly heard before. More often, those terms are borrowed from other domains and seem to hide very advanced concepts while, indeed, they encapsulate very simple ideas.

## A small definition ##

_Covariance_ and _contravariance_ come from mathematics. It is a measure of how changes in one variable are associated with changes in a second variable. It measures the degree to which two variables are linearly associated. In other words, it describes what happens when a change occurs. If the result goes in the same direction as the change, it is _covariant_, otherwise it is _contravariant_. 

We are not going to dig deeper with mathematical notions. We are rather going to take a look at how these concepts are translated in the world of computer science. First, we have to understand _Subtyping_.

## Subtyping ##

_Subtyping_ is a notion where a _subtype_, which is a data type, is related to a _supertype_. _Subtypes_ are an essential concept in object-oriented programming and are substitutable to _supertypes_. There is a difference between _Inheritance_ and _Subtyping_ and it can be defined like so:

* _Subtyping_ refers to compatibility of interfaces. A type _B_ is a _subtype_ of _A_ if every function that can be invoked on an object of type _A_ can also be invoked on an object of type _B_.
* _Inheritance_ refers to reuse of implementations. A type _B_ inherits from another type _A_ if some functions for _B_ are written in terms of functions of _A_.

In other words, _Subtyping_ refers to the compatibility of interfaces. _Inheritance_ refers to reuse of implementations. _Subtyping_ is about implementing an interface, and so being able to substitute different implementations of that interface at runtime. _Inheritance_ is about gaining attributes and functionalities of super types.

_Subtyping_ relation is defined by the type system of a programming language. A type system is a set of rules that assigns a type, which is a property, to the various constructs of a program (variables, expressions, functions, modules). The way to assign types is defined by type rules. A programming language has primitive data types. Those basic types are considered to be built using nullary type _constructors_, or, in other words, _constructors_ without any argument. A type _constructor_ is a feature that builds new types from old ones. A type _constructor_ is an _n-ary_ (number of arguments) type operator taking as argument zero or more types, and returning another type.

## Object hierarchies and substitution ##

_Covariance_ and _contravariance_ have a slightly different meaning in programming than in mathematics. The idea is related to the hierarchy of types.

Let's say we have two classes, _A_ and _B_. Let's say that _B_ is derived from type _A_. So, in other words, _A_ is the _base class_ for _B_. So _B_ contains every method and property that type _A_ does and more. So, we can say that _B_ is "bigger" than _A_. So, here we have a way of ordering classes. We can also say that _A_ is higher in the type hierarchy than _B_.

If _B_ derives from _A_, it means that it contains everything that _A_ contains. So, we can use _B_ anywhere that we could have used _A_. Of course, we can't always use _A_ in place of _B_. The relationship is asymmetrical. This is formally known as the _Liskov Substitution principle_. This principle also states that everything that is true for _A_ should be true for _B_.

## Formal definitions ##

Knowing what we know, let's try to elaborate some formal definitions.

A typing rule or a type _constructor_ is:

* _covariant_ if it preserves the ordering of types, which orders types from more specific to more generic
* _contravariant_ if it reverses this ordering
* _bivariant_ if it is _covariant_ and _contravariant_ at the same time
* _invariant_ or _nonvariant_ if it is neither _covariant_ or _contravariant_

So, we can say that _Covariance_ and _contravariance_ are about very specific situations, where we can treat one type as if it were another type in a certain context. _Covariance_ allows a "bigger", or less specific, type to be substituted in an _API_ where the original type is only used in an "output" position. _contravariance_ allows a "smaller", or more specific, type to be substituted in an _API_ where the original type is only used in an "input" position. _Covariance_ and _contravariance_ are the ability of a programming language to take advantage of commonalities between generic types deduced from known commonalities of their type arguments.

## Examples ##

Let's try to use examples to illustrate a little more what we have seen until now. Let's imagine that we have the following class:

    public class Person
    {
        public string Name { get; set; }
    } 

Now, we have a _derived class_:

    public class Client : Person { } 

Let's now imagine the following classes: 

    public class MailingList
    {
        public void Add(IEnumerable<out Person> persons) { }
    }

    public class Company
    {
        public IEnumerable<Client> GetClients() { }
    }

We can now make the following operations:

    var clients = company.GetClients();
    var mailingList = new MailingList();
    mailingList.Add(clients);

Here, we use a more derived type, _Client_, with the _Add()_ method. So, this last method is _covariant_. _Covariance_ allows us to pass a derived type where a base type is expected.

Let's now try to illustrate _contravariance_. Let's imagine the following _base class_ and _derived class_:

    class A { }
    class B : A { }

Suppose that we have a _delegate_:

    public delegate A MyDelegate(B b);

Now can imagine the following code:

    class Program
    {
        static A Method1(A a)
        {
            Console.WriteLine("Method 1");
            return new A();
        }

        static B Method2(B b)
        {
            Console.WriteLine("Method 2");
            return new B();
        }

        static void Main(string[] args)
        {
            MyDelegate myDelegate = Method1;
            myDelegate(new B());
        }
}

_contravariance_ allows a method with the parameter of a _base class_ to be assigned to a _delegate_ that expects the parameter of a _derived class_.

## Conclusion ##

Through this article we took a look at the concepts of _Covariance_ and _contravariance_. We saw that those terms were borrowed from mathematics and are relative to _Subtyping_, more specifically to object hierarchies and substitution. We saw that _Covariance_ allows us to pass a derived type where a base type is expected. On the other hand, _contravariance_ allows a method with the parameter of a _base class_ to be assigned to a _delegate_ that expects the parameter of a _derived class_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!