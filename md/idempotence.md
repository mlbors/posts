# Idempotence #

Through this article, we are going to take a look at the meaning of the word "_idempotence_" in programming.

## Introduction ##

The term "_idempotence_" could seem impressive, but in fact, the concept behind this big word is not really difficult to understand. Let's see how it is related to programming.

## Definition ##

_Idempotence_ is a concept borrowed from mathematics. It describes the property of certain operations that can be applied multiple times without changing the result beyond the initial application.

A unary operation, or an operation with one operand, is _idempotent_ if, whenever it is applied twice or more to any value, it returns the same results as if it were applied once.

A binary operation, an operation with two operands, has an _idempotent_ element if the corresponding given value, when it is assigned to both operands, is returned as a result after the operation.

A binary operation can also be _idempotent_ when it is applied to two equal values and gives that value as a result.

## Programming examples ##

We will frequently find such a thing when we look for information about _idempotence_:

    f(f(x)) = f(x)

Let's see how we can illustrate that. We can imagine the following function:

    // f(x)
    public function addTen(n)
    {
        return n + 10;
    }

Let's use our function:

    echo addTen(10); // Prints: 20
    // f(f(x))
    echo addTen(addTen(10)); // Prints: 30

As we can see, we get a different result each time we apply the function. So, this is not _idempotent_.

Now, consider the following example:

    echo abs(-10); // Prints 10
    echo abs(abs(-10)); // Prints 10
    echo abs(abs(abs(-10))); // Prints 10

This is _idempotent_, because we can apply the function as many times as we want and we always get the same result.

## More examples ##

We can have other things that are _idempotent_. For example, looking for a specific customer in a database is _idempotent_, because the database will not change. Changing the customer's address is also _idempotent_ because the final address will be the same no matter how many times it is submitted. Nevertheless, if the customer places an order, this operation is not _idempotent_ because several invocations will lead to several orders.

Make an _HTTP GET request_ is _idempotent_ because we can execute the same request how many times as we want, the result will remain consistent. However, making an _HTTP POST request_ is not _idempotent_ because it is designed to change the targeted element.

Sorting a list is also _idempotent_. If we sort the list once, then apply the function one more time, the order of the element won't change.

## Conclusion ##

Through this article we saw what the words "_idempotence_" means. We saw that it describes the property of certain operations that can be applied multiple times without changing the result beyond the initial application.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
