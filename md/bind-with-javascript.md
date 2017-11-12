# Bind with JavaScript #

In this article we are going to see how the _bind()_ method works in _JavaScript_ and how we can use it.

## Introduction ##

The _bind()_ method was introduced by _ECMAScript 5_. It creates a new function that, when called, has its _this_ keyword set to the provided value. It allows us to set which specific object will be bound to _this_ when a function or method is invoked. In other words, we pass our desired context into the _bind()_ function. When the callback function is executed, it uses the context we passed.

In simple and short terms, it solves the famous problem where we lose _this_ and, to avoid that, we place _this_ into a variable to reuse it later. It allows us to set which object is treated as _this_ within a function call.

Unfortunately, some old browsers don't support _bind()_. So, maybe we will have to keep using the "cache method" or use the method that a framework provides. Nevertheless, [MDN also suggests a solution with polyfill](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

## A simple example ##

Knowing what we know, let's make a simple example.

The following thing doesn't work:

	let character = {
		name: 'Mario',
		getName: function() {
			return this.name;
		}
	};

	let saySomething = function(sentence) {
		console.log("It's me, " + this.getName());
		console.log(sentence)
	}

	saySomething('Here we go!');

We can solve our problem by using the _bind()_ method like so:

	let character = {
		name: 'Mario',
		getName: function() {
			return this.name;
		}
	};

	let saySomething = function(sentence) {
		console.log("It's me, " + this.getName());
		console.log(sentence)
	}

	let logCharacter = saySomething.bind(character);
	logCharacter('Here we go!');

Here, we use the _bind()_ method by passing _character_ as an argument. So, _character_ is used as our context (_this_) with the copy of _saySomething_ and so it recognizes the properties and methods of _character_.

## Asynchronous ##

Let's imagine the following code:

	let character = {
		name: 'Mario',
		hello: function() {
			console.log(`It's me, ${this.name}!`);
		}
	};

	setTimeout(character.hello, 500);

We can see that it doesn't work because we have lost the context. We can solve the problem by using _bind()_.

	let character = {
		name: 'Mario',
		hello: function() {
			console.log(`It's me, ${this.name}!`);
		}
	};

	setTimeout(character.hello.bind(character), 500);

## Conclusion ##

Through this small article, we saw how _bind()_ works in _JavaScript_. We saw that it creates a copy of a function that uses the context that we passed in argument.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!