# Architectural Styles and Architectural Patterns #

Through this article, we are going to take a look at what we call _Architectural Styles_ and _Architectural Patterns_.

## Introduction ##

In software engineering, an _Architectural Pattern_ is a general and reusable solution to an occurring problem in a particular context. It is a recurring solution to a recurring problem.

The purpose of _Architectural Patterns_ is to understand how the major parts of the system fit together and how messages and data flow through the system.

We can have multiple patterns in a single system to optimize each section of our code.

## Architectural Patterns VS Design Patterns ##

_Architectural Patterns_ are similar to _Design Patterns_, but they have a different scope. In a few words, while _Design Patterns_ impact a specific section of the code base, _Architectural Patterns_ are high-level strategies that concern large-scale components, the global properties and mechanisms of a system.

## Styles and Patterns ##

Until now, we have talked about _Architectural Patterns_, but we can also talk about _Architectural Styles_. The main difference is, an _Architectural Pattern_, as we said, is a way to solve a recurring architectural problem, while an _Architectural Style_ is a name given to a recurrent Architectural Design. It doesn't exist to solve a problem.

A single architecture can contain several _Architectural Styles_, and each _Architectural Style_ can make use of several _Architectural Patterns_. An Architecture Patterns can be a subset of an _Architectural Styles_ targeting a specific scope.

We can use the same words used by the Building Architecture domain, where an _Architectural Style_ is characterized by the features that make a building notable and historically identifiable. A style may include such elements as form, a method of construction or building materials.

## Idiom ##

_Idiom_ is also a term that we can regularly meet. An _Idiom_ is a low-level pattern specific to a programming language. It describes how to implement particular aspects of the components or the relationships between them using the features of a given language. 

## Some major Architectural Patterns and Architectural Patterns Styles ##

Knowing what we know, let's now have a brief overview of some major _Architectural Patterns_ and _Architectural Styles_.

### Layered ###

This pattern is used to structure programs that can be decomposed into groups of subtasks. It partitions the concerns of the application into layers.

Most of the time, we have four layers:

* Presentation layer or UI layer
* Application layer or Service layer
* Business logic layer or Domain layer
* Data access layer or Persistence layer

The popular _Model-View-Controller structure_ (_MVC_) is a _Layered_ architecture. The _Model_ layer is just above the database and it sometimes contains some business logic. The _View_ is the top layer and corresponds to what the final user sees. The _Controller_ layer is in the middle and it is in charge to send data from the _Model_ to the _View_ and vice versa.

One major advantage of this pattern is the separation of concerns. It means that each layer focuses only on its role.

### Event-Driven ###

Also called EDA, this pattern organizes a system around the production, detection and consumption of events. Such a system consists of event _Emitters_ and event _Consumers_. _Emitters_ are decoupled from _Consumers_, which are also decoupled from each other.

An _Emitter_ is an event source and only knows that the event has occurred. A _Consumer_ needs to know an event has occurred and it has the responsibility of applying a reaction as soon as an event is presented. Sometimes, the reaction is not completely provided by a single _Consumer_ that might forward the event to another component after it has filtered or transformed it.

_Consumers_ can subscribe to an event manager receives notifications when events are emitted and forward events to all registered _Consumers_.

Event-driven architecture is easily adaptable to complex environments and can be easily extended when new event types appear. On the other hand, testing can be complex because interactions between modules can only be tested in a fully functioning system.

That kind of architecture is often used for asynchronous systems or user interfaces.

### Domain Driven Design ###

This _Architectural Style_, also known has _DDD_, is an object-oriented approach. Here, the idea is to design software based on the _Business Domain_, its elements and behaviors, and the relationships between them.

The _Business Domain_ is like a sphere of knowledge and activity around which the application logic revolves. It involves rules, processes and existing systems that need to be integrated into our solution. It is a set of classes that represent objects in the _Business Model_ being implemented. The _Business Model_ is the solution to the problem we are trying to solve. To organize and structure the knowledge of our problem, we use a _Domain Model_ that should be accessible and understandable by everyone who is involved with the project. _Domain Driven Design_ is about solving the problems of an organization. The _Domain Model_ is about understanding and interpreting the important aspects of the given problems.

A language is also structured around the _Domain Model_ and used by all team members to connect all the activities of the team with the software. It is called _Ubiquitous Language_. We also refer to the _Context_ to define the setting that determines the meaning of a statement.

_Domain Driven Design_ eases communication and improves flexibility.

_Domain Driven Design_ is useful when we build complex software where the need for change is determined. We have to be careful and remember that _DDD_ is not about how to code, but it is a way of looking at things.

### Pipes and Filters ###

This _Architectural Style_ decomposes a task that performs complex processing into a series of separate elements that can be reused. In other words, it consists of any number of components, called _Filters_, that transform or filter data, before passing it to other components through connectors called _Pipes_.

A _Filter_ transforms the data it receives through _Pipes_ with which it is connected. A _Filter_ can have many input _Pipes_ and many output _Pipes_.

A _Pipe_ is some kind of connector that passes data from one _Filter_ to the next.

There are also two other components, the _Pump_, which is the data source, and the _Sink_, which is the final target.

_Pipes and Filters_ can be applied when the processing of our application can be broken down into a set of independent steps. It can also be useful when flexibility is required or when each step of the processing of the application have different scalability requirements.

### Microservices ###

The goal of a _Microservices_ architecture is, instead of building one single big monolithic application, to create several tiny programs.

Such an architecture requires every service to be completely independent of the others.

This architecture can be helpful when we want to develop new businesses or web applications rapidly.

## Conclusion ##

Through this article we saw what _Architectural Patterns_ are. We compared them to _Architectural Styles_ and _Design Patterns_ to understand the differences. We also had a brief overview of some major _Architectural Patterns_ and Styles.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!