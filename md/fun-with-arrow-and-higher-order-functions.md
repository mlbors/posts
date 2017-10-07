# Fun with arrow and higher order functions

Through this small article, we are going to overview _arrow functions_ and _higher order functions_ in _JavaScript_.

## Arrow functions

In _JavaScript_, _arrow functions_ were introduced by _ECMAScript 6_ and are, in a way, a shorter manner to write functions. They allow us to write functions that are small, inline and single-purpose. They have a concise syntax, implicit returns and share lexical “_this_” with the parent scope.

_Arrow functions_ allow us to do something like this:
    
    
    //*************************************//
    //***** NOT USING ARROW FUNCTIONS *****//
    //*************************************//
    
    var sayHello = function() {
      console.log('Hello world!');
    }
    
    var splitePhrase = function(phrase) {
      return phrase.split(' ');
    }
    
    var multiply = function(x, y) {
      return x * y;
    }
    
    var setObject = function setProperties(id, name) {
      return {
        id: id,
        name: name
      };
    };
    
    //*********************************//
    //***** USING ARROW FUNCTIONS *****//
    //*********************************//
    
    let sayHello = () => { console.log('Hello world!'); }
    
    let splitePhrase = phrase => phrase.split(' ');
    
    let multiply = (x, y) => x * y;
    
    let setObject = (id, name) => ({id: id, name: name}); 

The side effect of _arrow functions_ is, because they are anonymous, even if we place them in variables, they will not give us very good stack traces. The other thing is we can not use them as constructors because they don’t have a prototype property or other internal methods.

## Higher order functions

A _higher-order function_ is a function that can take another function as an argument, or that returns a function as a result. The fact that we can place a function into another function allows us to compose small functions into bigger functions. The function passed to the other function is also known as _callback_. Let’s have some examples:

**Filter**

The _filter_ function returns an array filled with all elements of a provided array that had passed a specific test.
    
    
    const animals = [
      { name: 'Spike', species: 'dog' },
      { name: 'Garefield', species: 'cat' },
      { name: 'Tom', species: 'cat' },
      { name: 'Droopy', species: 'dog' }
    ];
    
    var isCat = (function(animal) {
      return animal.species === 'cat';
    });
    
    var cats = animals.filter(isCat);
    
    console.log(cats);
    
    /**
     * Will output
     * Array(2)
     * 0 : "Garefield"
     * 1 : "Tom"
     */

**Map**

We can use _map_ if we want to create an array with the values of another array after each of them went through a specific function.
    
    
    const persons = [
      { name: 'John', browser: 'Chrome' },
      { name: 'Jane', browser: 'Firefox' },
      { name: 'Sam', browser: 'Chrome' },
      { name: 'Paul', browser: 'Edge' }
    ];
    
    var names = persons.map(function(person) {
      return person.name;
    });
    
    console.log(names);
    
    /**
     * Will output
     * Array(4)
     * 0 : "John"
     * 1 : "Jane"
     * 2 : "Sam"
     * 3 : "Paul"
     */

**Reduce**

The _reduce_ function reduces an array to a single value. It executes a provided function for each value of the array, from left to right.
    
    
    var orders = [
      { amount: 210 },
      { amount: 35 },
      { amount: 142 },
      { amount: 47 }
    ];
    
    var totalAmount = orders.reduce(function(sum, order) {
      return sum + order.amount
    }, 0);
    
    console.log(totalAmount);
    
    /**
     * Will output
     * 43
     */

## Let’s have fun

Now, let’s try to put all this information together. For some reasons, let’s say we are working on a brand new video game where some Italian plumbers have to rescue a princess from a dangerous dragon monster that breathes fire (any resemblance to fictional famous characters is purely coincidental).

Down bellow, we have an array with some events concerning the big monster:
    
    
    const bigMonsterEvilDragonEvents = [
      { type: 'attack', value: 12, target: 'italian-plumber-1' },
      { type: 'attack', value: 12, target: 'italian-plumber-2' },
      { type: 'fire', value: 40 },
      { type: 'jump', target: 'bridge' },
      { type: 'attack', value: 23, target: 'italian-plumber-1' },
      { type: 'jump', target: 'tower' },
      { type: 'laugh', value: 95 },
      { type: 'attack', value: 6, target: 'italian-plumber-2' },
      { type: 'attack', value: 15, target: 'italian-plumber-1' }
    ];

If we want to know how much damage the big monster made on the Italian Plumber 1 at the end of all of these events, using what we previously saw, we can simply do it like this:
    
    
    const totalDamageOnItalianPlumber1 = bigMonsterEvilDragonEvents
    .filter(e => e.type === 'attack')
    .filter(e => e.target === 'italian-plumber-1')
    .map(e => e.value)
    .reduce((prev, x) => (prev || 0) + x);
    
    console.log(totalDamageOnItalianPlumber1);
    
    /**
     * Will output
     * 50
     */

That’s it! That is how we can use _arrow functions_ and _higher order functions_ to easily have access to some results that would require more lines of code without these last functions.
