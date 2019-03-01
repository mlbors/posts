# What are JavaScript Proxies? #

In this post, we are going to see what _JavaScript Proxies_ are, how they work and how we can use them.

## Introduction ##

_Proxies_ were introduced in _ES6_ and are useful to define a custom behaviour for fundamental operations. In other words, a _Proxy_ is an object that stands between an object and what we could call the outside world. It means that we can wrap an existing object and intercept any access to its attributes or its methods.

## How it works? ##

Three components are important when we talk about _Proxies_:

* **Target:** the object that will be wrapped (it can be any sort)
* **Traps:** the methods that provide property access 
* **Handler:** the placeholder object that which contains _Traps_

As we said before, we can use a _Proxy_ to define a custom behaviour whenever the properties or the methods of an object, the _Target_, are accessed. It allows us to provide a custom functionality to a basic operation that can be performed on an object. We achieve this by using _Traps_. The _Handler_ object passes the _Target_ and the requested element to the concerned _Trap_. A complete list of the various _Traps_ can be found [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy#Methods_of_the_handler_object).

## A simple example ##

Let's start with a simple example. First, we define an object

    const mario = {
        name: 'mario',
        profession: 'plumber'
    }

Now, let's do the following things:

    console.log(mario.name) // output: mario
    console.log(mario.profession) // output: plumber
    console.log(mario.power) // output: undefined

As we can see, our object has only two properties and we want to access a third one that doesn't exist. Sadly, we receive "_undefined_" in that case. How can we return a default value instead when we try to access a property that doesn't exist? We can do this by using the _GET Trap_. We can now define a _Handler_ and a _Proxy_:

    const handler = {
        get: function(target, name) {
            return name in target ? target[name] : 'none'
        }
    }

    const p = new Proxy(mario, handler)

Let's try to access the properties of our object through the _Proxy_:

    console.log(p.name) // output: mario
    console.log(p.profession) // output: plumber
    console.log(p.power) // output: none

## When to use a Proxy? ##

We can imagine using a _Proxy_ when we want to enforce value validation in _JavaScript_ object. We can, for example, simply check if the value we want to set is correct or if the affected property can be modified.

We can also use a _Proxy_ to revoke the access to an object or simplify a data structure querying process.

## Conclusion ##

Through this brief article, we saw what _JavaScript Proxies_ are. We saw that a _Proxy_ is an object that is used to define custom behaviour for fundamental operations. There are three important terms when we talk about _Proxies_: _Target_, _Traps_ and _Handler_. 

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!