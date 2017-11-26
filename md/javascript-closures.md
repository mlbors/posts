# JavaScript Closures #

_Closures_ are really common in _JavaScript_, but their definition can seem to be a little dark. Let's take a look at them!

## Introduction ##

_MDN web docs_ defines _closures_ like so:

"_A closure is the combination of a function and the lexical environment within which that function was declared._"

What does that really mean? We can view _closures_ as objects that contain a function and a reference to the environment in which the function was created. They allow to implement such things as _callbacks_ or _event handlers_. They give us access to an outer function's scope from an inner function.

Basically, we can use a _closure_ by defining a function inside another function. We then have to expose it by returning it or passing it to another function. 

In _JavaScript_, _closures_ are used to enable data privacy because the enclosed variables are only in scope within the parent function.

## A few definitions ##

First, let's take a look at some definitions.

### Scope ###

In _JavaScript_, a scope refers to the "_place_" where variables and functions are accessible and in what context it is being executed. When something is accessible from anywhere in our code, it is in the global scope. Local scope refers to something that is only accessible in a certain part of our code.

When we create a function, it has access to variables created inside and outside the function. Variables created inside a function are locally defined and can only be accessed inside this function.

### First-Class Functions ###

Basically, functions are considered as _First-Class Citizens_ when they are "_being able to do what everyone else can do_". So, they can be assigned to a variable, passed as an argument or returned from a function.

### Inner Functions ###

Also called _Nested Functions_, they are defined inside of another function (outer function). Every time this outer function is called, an instance of the inner function is created.

## Basic example ##

Knowing what we know, let's make a simple example using a _closure_.

	function say() {
		let x = 'Hello World!';  
		function hello() {  
			console.log(x);    
		}
		return hello;
	}

	let sayHello = say();
	sayHello();

Here, our function _say()_ returns the inner function _hello()_ from the outer function before being executed. The _hello()_ function is only available within the body of the _say()_ function and has no local variables. Nevertheless, because inner functions have access to the variables of outer functions, _hello()_ can access the variable _x_ declared in the parent function.

Once _say()_ has finished executing, we might expect that the _x_ variable would no longer be accessible, but it is not the case. _sayHello()_ is a reference to the instance of the function _hello()_ that is created when _say()_ function is run and so a reference is kept to the lexical environment of _hello()_ where the variable _x_ exists. This is why this last variable remains available when _sayHello()_ is invoked.

## A factory of functions ##

Let's imagine the following code:

	function sentenceFactory(x) {
		return function(y) {
			return String(x) + String(y);
		};
	}

	let sentence1 = sentenceFactory('Hello ');
	let sentence2 = sentenceFactory('Hey ');

	console.log(sentence1('John'));
	console.log(sentence2('Paul'));

Here, we create some kind of factory, _sentenceFactory(x)_, that takes a single argument and returns a new function that also takes one argument. Our functions _sentence1_ and _sentence2_ are closures and they share the same function body definition, but they store different lexical environments.

## Controlling side effects ##

_Closures_ can be helpful to control side effects, such as _Ajax request_ or _timeout_.

	function sayHello(name) {
		setTimeout(function() {
			console.log('Hello ' + name);
		}, 1000);
	}

The previous code would print out our message immediately after one second. Maybe we don't want that and print our message later. We can do that with a _closure_:

	function sayHello(name) {
		return function() {
			setTimeout(function() {
				console.log('Hello ' + name);
			}, 1000);
		}
	}

	const sayHelloLater = sayHello('Ringo');
	sayHelloLater();

## Emulating private methods ##

_JavaScript_ does not support private data and does not provide a native way of doing this. However, we can emulate private methods using _closures_. Private methods are useful for restricting access to code and provide a way of managing our global namespace.

	function Person(name) {

		let _name = name;
		
		function takeInstrument() {
			return 'guitar';
		}
		
		return {
			sayHello: function() {
				console.log('Hello ' + _name);
			},
			getName: function() {
				console.log(_name);
			},
			play: function() {
				console.log(_name + ' is playing the ' + takeInstrument());
			}
		}

	}

	const person1 = Person('George');
	person1.sayHello();
	person1.getName();
	person1.play();

In this example, _name_ and _takeInstrument()_ are private because they cannot be accessed directly from outside the _Person(name)_ function. They are accessed by three public functions that are returned.

## Loop problem ##

It can be very tempting to create _closures_ in a loop. Let's first imagine the following example:

	function generateLinks() {

		var info = [
			{'name': 'John', 'instrument': 'Guitar'},
			{'name': 'Paul', 'instrument': 'Bass'},
			{'name': 'George', 'instrument': 'Guitar'},
			{'name': 'Ringo', 'instrument': 'Drum'}
		];

		for (var i = 0, link; i < info.length; i++) {
		
			var item = info[i];
		
			var link = document.createElement("a");
			link.innerHTML = item.name;
			link.onclick = function() {
				alert(item.instrument);
			};
			
			document.body.appendChild(link);
		}
		
	}

	window.onload = generateLinks;

This code doesn't work as expected: our alert always shows the same message. But why? Four _closures_ have been created by the loop, but each one shares the same single lexical environment. The _item.instrument_ value is determined when the _onclick callbacks_ are fired. It means that the loop continues until the end and so the _item_ variable is left pointing to the last entry when _generateLinks()_ exits.

We could fix it like so:

	function createAlert(info) {
		return function() {
			alert(info);
		};
	}

	function generateLinks() {

		var info = [
			{'name': 'John', 'instrument': 'Guitar'},
			{'name': 'Paul', 'instrument': 'Bass'},
			{'name': 'George', 'instrument': 'Guitar'},
			{'name': 'Ringo', 'instrument': 'Drum'}
		];

		for (var i = 0, link; i < info.length; i++) {
		
			var item = info[i];
		
			var link = document.createElement("a");
			link.innerHTML = item.name;
			link.onclick = createAlert(item.instrument);
			
			document.body.appendChild(link);
		}
		
	}

	window.onload = generateLinks;

We could also fix our code by using _let_ instead of _var_.

## In other programming languages ##

Closures are not available just in _JavaScript_. We can use them in other programming languages.

With _PHP_:

	function adder($n) {
		return function($v) use($n) {
			return $v + $n;
		};
	}

	$adder10 = adder(10);
	$adder10(1);

With _Python_:

	def adder(nombre):
		return lambda v: v + n

	adder10 = adder(10)
	adder10(1)

With _Swift_:

	func adder(_ n: Int) -> (Int) -> Int {
		return { n + $0 }
	}

	let adder10 = adder(10)
	print(adder10(5))

And so on!

## Conclusion ##

Through this article, we took a look at _closures_ in _JavaScript_ and saw how they work. We saw that a _closure_ is formed when an outer function exposes an inner function and it contains a function and a reference to the environment in which it was created. _Closures_ can be used to deal with _callback_ functions and to emulate private data.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!