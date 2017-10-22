# When JavaScript Promises #

Through this article, we are going to see what _Promises_ are in _JavaScript_ and how we can use them.

## Introduction ##

_Promises_ are now widely used in _JavaScript_, but they can stay a little confused. _Promises_ were introduced by _ECMAScript 6_. They intend to provide a way to deal with the completion of asynchronous tasks. They are an alternative to _callbacks_ for delivering the results of an asynchronous computation.

Just for a simple refresh, a _callback_ function, also known as a _higher-order function_, is a function that is passed to another function as a parameter. The function is called inside this other function.

One of the biggest problems of _callbacks_ is the chaining of different asynchronous activities, because we end up with a growing stack of _callback functions_ (callback hell). _Promises_ aim to provide a solution to this problem and an alternative for executing, composing and managing asynchronous operations.

## How it works ##

A _Promise_ is an object that can be returned synchronously from an asynchronous operation and it can have one of the following states:

* Pending: we don't know the outcome because the operation that will produce the result hasn't completed yet.
* Fulfilled: the operation has completed and we have a value
* Rejected: the operation failed

A _Promise_ is _settled_ if it’s not _pending_.

## Resolve, Reject ##

The _Promise constructor_ takes one argument: a _callback function_ with two parameters, _resolve_ and _reject_. It is our responsibility to call _resolve_ and _reject_, and eventually to pass values to them.

Let's make a very basic example with what we know:

    const p => time = new Promise((resolve, reject) => {
        if (time < 1) {
            reject('Failure');
            return;
        }
        
        setTimeout(resolve('Success'), time);
    });

In this example, we just create a _Promise_ that will return '_Success_' after a given time or will fail if the given time is smaller than 1. Here, we placed a _return_ after the _reject_ to terminate the control flow and to prevent the execution of extra code, but it is not strictly needed.

## Then / Catch ##

How can we use the _Promise_ that we created? We can do it with the _then method_. This method takes two arguments, _resolved_ and r_ejected_. This last one is optional and is called when the _Promise_ is rejected. We could do it like so:

    p(500).then((resolved) => {
        console.log(resolved);
    }, (rejected) => {
        console.log(rejected);
    });

We can also use the _catch method_ to handle errors.

    p(500).then((resolved) => {
        console.log(resolved);
    }).catch((error) => {
        console.log(error);
    });

In case of error, we can also use _throw_. We can use it any time we are inside a _Promise callback_, but if we are in any other asynchronous _callback_, we have to use _reject_ because otherwise _catch_ won't be triggered and we will be left with an unresolved _Promise_ and an uncaught exception.

    // Uncaught exception

    const p = new Promise((resolve) => {
        setTimeout(() => {
            throw 'Error';
        }, 500);
    });

    p.then((res) => {

    }).catch((error) => {
        console.log(error); // won't happen
    });

    // Caught exception

    const p = new Promise((resolve) => {
        setTimeout(() => {
            resolve('Ok');
        }, 500);
    });

    p.then((res) => {
        throw 'error';
    }).catch((error) => {
        console.log(error);
    });

## Chaining ##

The _then method_ returns a _Promise_ which allows for method chaining.

    p(500).then((val) => {
        console.log(val);
        return val;
    }).then((val) => {
        val = val + ' ' + 'foo';
        console.log(val);
    }).catch((error) => {
        console.log(error);
    });

## Promise.all() ##

The _Promise.all_ method returns a single _Promise_ that resolves when all of the _Promises_ passed in the argument have resolved. So, this method takes an array of _Promises_ and fires one _callback_ once they are all resolved. It can be used to aggregate the results of multiple _Promises_.

    const p = time => new Promise((resolve, reject) => {
        if (time < 1) {
            reject('Failure');
            return;
        }
        
        setTimeout(resolve('Success'), time);
    });

    const p2 = new Promise((resolve, reject) => {	
        setTimeout(resolve('Another promise'), 1000);
    });

    p(500).then((val) => {
        console.log(val);
        return val;
    }).then((val) => {
        val = val + ' ' + 'foo';
        console.log(val);
        return Promise.all([p2, val]);
    }).then(([p2, val]) => {
        console.log(val + ' ' + p2);
    }).catch((error) => {
        console.log(error);
    });

## Conclusion ##

In this article, we saw what _Promises_ are and how we can use them. We saw that _Promises_ are an alternative to _callbacks_ for delivering the results of an asynchronous computation.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!