# Memoization #

Through this article, we are going to look at "_memoization_".

## Introduction ##

The word "_memoization_" seems to be misspelled, but in fact it is not. It is an optimization technique to speed up a program. Let's see how it works.

## Definition ##

The word "_memoization_" seems to be derived from the _Latin_ word "_memorandum_" ("to be remembered"). This technique consists, for a given function, in storing previously computed results to increase performances. The idea is to remember the computed result for certain inputs and to use the memorized result, instead of recomputation if these specific inputs get used once again.

We can see _memoization_ as some kind of _caching_ technique, but there is a tiny difference: with _memoization_, which is a specific form of _caching_, we store the returned value of a function based on its parameters. With _caching_, we speak of a more general domain that includes any _output-buffering strategy_.

## Example ##

Let's make an example using what we know. Consider the following function:

    const factorial = n => {
        return n === 0 || n === 1 ? 1 : n * factorial(n-1)
    }

We can see this is a factorial function. However, calling this function multiple times for the same value of "_n_" can damage performances. That's where we can make use of _memoization_. Let's modify our function like so:

    const factorial = n => {
        let cache = {}
    
        if (n in cache) {
            return cache[n]
        }

        if (n === 0 || n === 1) {
            return 1
        }
    
        return cache[n] = n * factorial(n-1)
    }

As we can see, now, in our function, we store the different computed values in a really simple manner.

Now, let's go a little further. What if we want to use _memoization_ with multiple functions?

    const memoize = f => {
        let cache = {}
        
        return (args) => {
            let n = arg
            if (n in cache) {
                return cache[n]
            }
            return cache[n] = f(n)
        }
    }

    const factorial = memoize(n => n === 0 || n === 1 ? 1 : n * factorial(n-1))
    const fibonacci = memoize(n => n === 0 || n === 1 ? n : fibonacci(n-1) + fibonacci(n-2))

Here, we now have a "_memoize_" function that wraps any function to create a "_memoized_" version. We have to notice that we only manage one argument.

## Conclusion ##

Through this article, we saw the meaning of the word "_memoization_" and how, here with _JavaScript_, we can use such a concept.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
