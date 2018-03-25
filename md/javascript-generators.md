# JavaScript Generators #

In this article, we are going to take a look at _Generators_ in _JavaScript_.

## Introduction ##

_JavaScript Generators_ were introduced by _ES6_. It is a type of function that can be entered and exited a number of times. Let's see what it really is. 

## Definition ##

When a standard _JavaScript_ function is called, it runs until it reaches a return value or the end of the function. _Generators_ allow us to run just one part of a function, then exit it to come back later and restart it exactly where we left. They give us the control over the flow of a function.

To pause such a function, we have to use the "_yield_" keyword inside this function. The function will be exited. To re-enter the same function, we have to use the "_next()_" method. Each time we exit a function, we can gather some information. We have access to the value that was possibly returned and the status of the _Generator_ function. 

## Example and explanation ##

Let's make a simple example of a _Generator_ function.

    function* mario() {
        console.log("It's-a Me, Mario!");
        console.log("Jumping");
        console.log("Eating a mushroom")
        yield "pause";
        console.log("Running");
        yield "pause";
        console.log("Falling into lava");
        yield "game over";
    }

    const marioValues = mario();
    let gen;

    gen = marioValues.next(); // Output: It's-a Me, Mario!; Jumping; Eating a mushroom
    console.log(gen.value, gen.done); // Output: pause false

    gen = marioValues.next(); // Output: Running
    console.log(gen.value, gen.done); // Output: pause false

    gen = marioValues.next(); // Output: Falling into lava
    console.log(gen.value, gen.done); // Output: game over false

    gen = marioValues.next(); // Output: -
    console.log(gen.value, gen.done); // Output: undefined true

As we can see, we declare a _Generator_ by using the "_\*_" symbol. Here, we have three "_yield_" statements and we can see that each time one of them is reached, the function is exited.

We can notice that the "_done_" value turns to "_true_" after we called the "_next()_" method for the fourth time. At this time only, the function is complete and the "_value_" turns to "_undefined_". We can correct this by replacing the last "_yield_" by a "_return_". In this case, it will work. However, in the case of a "_for..of_" loop, the value returned by the "_return_" keyword would be thrown away.

When we call the _Generator_, the function is not executed. In fact, an iterator object is created that will let us interact with the _Generator_. As soon as the iterator is created, the _Generator_ goes into a suspended state. 

## Conclusion ##

Through this small article, we saw what _Generators_ are in JavaScript. We saw how to use them through a really basic example.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
