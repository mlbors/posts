# Restoring with the Memento Pattern #

In this article we are going to take a look at the _Memento Pattern_ and how we can use it. Let's get into it!

## Introduction ##

The _Memento Pattern_ is a _Behavioural Pattern_ that offers a way to restore an object back to its previous state, like an "_undo_" or a "_rollback_" operation. Its formal definition is the following one:

"_Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later._"

One of the most famous examples that illustrates the _Memento Pattern_ is a good old calculator where the last calculation is saved in memory so that we can get back to it and eventually restore it.

## The Pattern ##

The _Memento Pattern_ contains a snapshot of the state of an object in a way that it can be restored later without revealing its implementation. It is useful when we want to provide some sort of "_undo_" functionality or when using a direct interface to obtain the state would expose implementation details and break the object's encapsulation.

The _Command Pattern_ can use _Memento_ to maintain the state required for an "_undo_" operation.

## Structure ##

The _Memento Pattern_ needs three things:

### Memento ###

It contains a concrete snapshot of the state of any object. The state can include any number of state variables. The _Memento_ object is like a black box that no one can or should change. 

### Originator ###

It contains the actual state of an external object and knows how to save itself. So it can create a _Memento_ containing a snapshot of its current internal state and it can use the _Memento_ to restore its internal state. However, it doesn't know the history of changes.

### Caretaker ###

It is responsible for keeping the _Memento_ and it must not operate on it. It knows why and when the _Originator_ needs to save and restore itself.

## Example ##

Knowing what we know, let's make a quick example of this pattern. We are going to use _PHP_ for this and we are going to say that we are building a basic text editor.

First, we create the _Memento_:

	class Memento
	{

		protected $content;

		public function __construct(string $content)
		{
			$this->content = $content;
		}

		public function getContent()
		{
			return $this->content;
		}
		
	}

Then we can define our _Originator_. Here it is going to be our editor.

	class Editor

	{
		protected $content = '';

		public function type(string $words)
		{
			$this->content = $this->content . ' ' . $words;
		}

		public function getContent()
		{
			return $this->content;
		}

		public function save()
		{
			return new Memento($this->content);
		}

		public function restore(Memento $memento)
		{
			$this->content = $memento->getContent();
		}
		
	}

Our _Client_, or the _Caretaker_, can be like so:

	$editor = new Editor();
	$editor->type('A sentence.');
	$editor->type('Another sentence');

	$saved = $editor->save();

	$editor->type('And another one');

	$editor->restore($saved);

## Conclusion ##

Through this short post we saw how works the _Memento Pattern_. We saw this is a _Behavioural Pattern_ and it needs three things: a _Memento_, an _Originator_ and a _Caretaker_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
