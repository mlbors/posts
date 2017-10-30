# The Flyweight Pattern #

In this article we are going to take a look at the _Flyweight Pattern_ to understand how it works and how we can use it.

## Introduction ##

The _Flyweight Pattern_ is a _Structural Pattern_. Its intent is to minimize memory usage. The famous _Gang of Four_ defines it like so:

"_Use sharing to support large numbers of fine-grained objects efficiently. A Flyweight is a shared object that can be used in multiple contexts simultaneously. The flyweight acts as an independent object in each context; it’s indistinguishable from an instance of the object that’s not shared._"

There is a lot of information in this definition. So, let's illustrate it with a "real world" example: imagine that we are building a video game where a thousand characters have to fight on the battlefield. Each character is an object that contains, for example, a graphical representation, a behaviour, weapons and information about its location and its health. Creating such a number of objects will use a lot of memory. We can improve that by sharing the common information, such as graphical representation and behaviour. Health and location will, however, vary.

Now we have that in mind, let's dig a little deeper.

## The Pattern ##

The intent of the _Flyweight Pattern_ is to use sharing to support a large number of objects that have part of their internal state in common while the other part of state varies.

A common practice is to keep state in external data structures and pass them to the _Flyweight Object_ when needed. Each _Flyweight Object_ is divided into two pieces: the state-dependent (extrinsic) part, and the state-independent (intrinsic) part.

The intrinsic data is held in the properties of the _Flyweight Objects_ that are shared. _Intrinsic State_ is invariant, so it is _context_ independent. In that sense, intrinsic properties make objects "constant". The extrinsic data is context dependent. So it cannot be shared and must be passed in through arguments, but should never be stored within a shared _Flyweight Object_. In our video game example, the graphical representation would be the intrinsic data and the location and the health would be the extrinsic data.

This pattern is useful when we have a program using a large number of objects. This pattern is often combined with _Composite_ to implement shared leaf nodes.

## Structure ##

To apply this pattern, we need the following things:

### Flyweight ###

It declares an interface through which _Flyweights_ can receive and act on extrinsic state.

### Concrete Flyweight ###

It implements the _Flyweight interface_ and adds storage for intrinsic state

### Unshared Concrete Flyweight ###

It provides a way to use this pattern without enforcing the shared concept.

### Flyweight Factory ###

It creates and manages _Flyweight objects_ and ensures that _Flyweights_ are shared properly. It keeps track of the _Flyweights_ that already exist and when it is asked to create a new _Flyweight_, it checks if a matching _Flyweight_ already exists. If there is no object of the required type, it creates a _Flyweight_ of the requested type, adds it to the pool, and returns a reference to the _Flyweight_.

### Client ###

It maintains a reference to _Flyweights_, computes or stores the extrinsic state of those.

## Example ##

Let's take back our example with our video game and let's make a really basic implementation of this pattern with _Java_.

First, we define the _Flyweight interface_:

	public interface Character {
		public void render();
	}

We can now create a concrete implementation:

	public class CharacterA implements Character {

		private String color;
		private int x;
		private int y;

		public Circle(String color){
			this.color = color;		
		}

		public void setX(int x) {
			this.x = x;
		}

		public void setY(int y) {
			this.y = y;
		}

		public void render() {
			System.out.println("Character at position " + x + ", " + y + " with color " + color);
		}

	}

Let's make the _Factory_ that embeds a _Pool system_:

	public class CharacterFactory {

		private static HashMap<String, Character> characterMap = new HashMap();

		public static Character getCharacter(String color) {
			Character character = (Character)characterMap.get(color);

			if (character == null) {
				character = new CharacterA(color);
				characterMap.put(color, character);
			}

			return character;
		}

	}

And we can make our _Client_ like so:

	public class CharacterClient {

		private static final String colors[] = { "Red", "Green", "Blue", "White", "Black" };

		public static void main(String[] args) {
			for (int i = 0; i < 15; ++i) {
				Character character = (Character)CharacterFactory.getCharacter(getRandomColor());
				character.setX(getRandomX());
				character.setY(getRandomY());
				character.render();
			}
		}

		private static String getRandomColor() {
			return colors[(int)(Math.random() * colors.length)];
		}

		private static int getRandomX() {
			return (int)(Math.random() * 100);
		}

		private static int getRandomY() {
			return (int)(Math.random() * 100);
		}

	}

## Conclusion ##

In this article, we saw what the _Flyweight Pattern_ is and how it works. This pattern is used to minimize memory usage by sharing as much as possible with similar objects. We saw that it is based on the following things: _Flyweight_, _Concrete Flyweight_, _Unshared Concrete Flyweight_, _Flyweight Factory and Client_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!