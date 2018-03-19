# What is currying? #

Through this article, we are going to talk about "_currying_".

## Introduction ##

Currying has nothing to do with spices, unfortunately. In fact, this process is named after _Haskell Curry_, who was a mathematician. So, let's see what that means.

## Pure functions ##

So currying allows us to get _pure functions_. But what is a _pure function_? A function is considered as pure if:

* The function always evaluates the same result value given the same argument value(s)
* Evaluation of the result does not cause any semantically observable side effect or output

So for example, "_sin(x)_" is a _pure function_, because it returns the _sine_ of a number _x_. On the other side, something like a "_date()_" function is not pure because it returns something different every day.

A function that uses only its own arguments, its local variables and _pure functions_, is a _pure function_.

## Currying ##

So, now that we know what a _pure function_ is, let's look a little closer at the _currying_ process. _Currying_ is a way to reduce functions of more than one argument to functions of one argument. In other words, it allows us to break down a function into a series of functions that each have a single argument. The inverse process of _currying_ is _uncurrying_.

But why would we do such a thing? Because it helps us to avoid passing the same variable over and over again. It lets use create little pieces that can be configured and reused with ease.

## Examples ##

Let's see in practice what we saw means. Using _JavaScript_, let's say that we have the following function:

    function add (a, b) {
        return a + b;
    }

So, this function could be used like so:

    add(3, 5); // returns 8

So, we can use _currying_ and transform this function like this:

    function add (a) {
        return function (b) {
            return a + b;
        }
    }

As we can see, our function now has only one argument and returns another function. So, we could use it like so:

    add(3)(5); // returns 8

Now, let's say that we have to use the value "3" plenty of times. We can use the advantages of _currying_ like so:

    var add3 = add(3);
    add3(5); // returns 8
    add3(8); // returns 11
    add3(2); // returns 5

As we can see, we have a simple little "_piece_" named "_add3_" that we can reuse over and over. This process is called _partial application_. _Partial application_ is when we call a function with one or multiple arguments and it returns a function that still accepts arguments. 

## Conclusion ##

Through this article we saw what "_currying_" means and how we can use it. We saw the concepts that it is related to and the advantages of this process.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!