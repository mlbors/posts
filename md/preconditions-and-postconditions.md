# Preconditions and Postconditions #

In this article, we are going to talk about the terms "_precondition_" and "_postcondition_".

## Introduction ##

The words "_precondition_" and "_postcondition_" can seem to be frightening words, but they are not so complicated to understand. Let's see what they mean and how they are implied in programming.

## Definitions ##

First, let's define, in some kind a formal way, the words "_precondition_" and "_postcondition_".

### Precondition ###

A _precondition_ is a condition, or a predicate, that must be true before a method runs for it to work. In other words, the method tells clients, "this is what I expect from you". So, the method we are calling is expecting something to be in place before or at the point of the method being called. The operation is not guaranteed to perform as it should unless the _precondition_ has been met.

### Postcondition ###

A _postcondition_ is a condition, or a predicate, that can be guaranteed after a method is finished. In other words, the method tells clients, "this is what I promise to do for you". If the operation is correct and the _precondition(s)_ met, then the _postcondition_ is guaranteed to be true.

## Real world examples ##

Let's see, in the real world, what could be a _precondition_ and a _postcondition_.

* Eating a pizza - _precondition_: there is a pizza; _postcondition_: there is no pizza
* Withdrawing money from a debit account in ATM - _preconditions_: the sum being withdrawn should be smaller than or equal to the sum remaining on the account and smaller than the sum remaining in the ATM; _postconditions_: the reminders of money in both ATM and on the account should equal their original values minus the sum being withdrawn
* Mario beating Bowser - _precondition_: Bowser is here; _postcondition_: Mario saved Peach

## A more pragmatic example ##

Now, let's have a more pragmatic example. Let's imagine that we have a function that computes the square root of a number. So the _preconditions_ here are the argument to the square root function must be non-negative and greater than or equal to zero. The _postcondition_ is the square of the computed result must equal the argument. We could express it this way:

    def square_root(x):
        assert x >= 0 // _preconditions_

        THE COMPUTATION ITSELF... RETURNS A VALUE NAMED Y

        assert y*y == x // _postcondition_
        return y

## Programming example ##

Let's say we have the following piece of code:

    int getSum(int a, int b)
    {
        int sum = a + b;
        return sum;
    }

This function just computes the sum of two given integers and returns it. So, in this case, the _preconditions_ are the two given values are two valid integers. The _postcondition_ is that the sum of those integers is returned and is in fact, an integer.

## Invariant ##

We may sometimes hear the word "invariant". An invariant is something that is always true and won't change. The method tells clients, "if this was true before you called me, I promise it'll still be true when I'm done". An invariant is a combined _precondition_ and _postcondition_. It has to be valid before and after a call to a method.

## Conclusion ##

Through this article, we saw what "_precondition_" and "_postcondition_" mean. We saw that a _precondition_ is a condition that must be true before a method is called while a _postcondition_ is a condition that must be true after the method is complete.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
