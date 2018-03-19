# What is immutability? #

Through this article, we are going to talk about the concept of "_immutability_". Let's get into it!

## Introduction ##

"_Immutability_" is a word that we can hear a lot without knowing exactly what it is. We can even use what it refers to without knowing it. Let's see what it means.

## Definition ##

In functional and in object-oriented programming, an object is considered as _immutable_ when its _state_ can't be changed after it was created. The opposite of such an object is a _mutable_ object, which can be modified after its creation.

But what is the _state_ of an object? If, at a given time, for a given object, we wrote down on a piece of paper all the variables of this same object, we would get the _state_ of this object. A program also has a _state_ and this one corresponds to the _state_ of all its objects at a given time. But, to evolve, a program needs to change _state_ and that rapidly. So, an _immutable_ object has a fixed _state_ over time and even if the whole _state_ of the program changes, once created, the _state_ of such an object doesn't.

Now, we can have some cases where an object is considered as _immutable_ even if some of its attributes change because that object's _state_ remains unchanged from an external point of view.

## Benefits ##

But what are the benefits of _immutable_ objects? For example, they can be helpful in _multi-threaded_ applications where multiple _threads_ can use data represented by _immutable_ objects without concern of the data being changed by other _threads_.

_Immutable_ objects also provide a consistent _state_. It means that an _immutable_ object will always remain safe. For example, if we pass all the needed values to an object using the _constructor_ and not through _setter methods_, we can be sure that all the necessary data to have a valid object will always be present.

## Example ##

Let's now see a more practical example.

_Strings_ are considered as _immutable_. However, we can do the following thing without any problem:

    name = 'Mario'
    print name // Output: Mario

    name = 'Luigi'
    print name // Output: Luigi

_Immutability_ doesn't mean we can't reassign a variable, here "_name_", again. So, what is going on? Here, we don't modify our _String_ object. In fact, we create a new _String_ object. We could look at the memory address of our "_name_" variable after both assignments and we would see that the memory address is different.

Now, the following example would return an error because a _String_ is _immutable_:

    name = 'Mario'
    name[0] = 'W' // Output: error

The following example shows the use of a _mutable_ object:

    characters = ['Mario', 'Luigi', 'Peach', 'Bowser']
    print characters // Output: ['Mario', 'Luigi', 'Peach', 'Bowser']

    characters[0] = 'Wario'
    print characters // Output: ['Wario', 'Luigi', 'Peach', 'Bowser']

Here, we can do this because our _List_ object is _mutable_ and if we looked at the memory address of this object after our different operations, we would see that the memory address stays the same.

Now, just to cogitate a little bit, let's consider the following case:

    characters = ['Mario', 'Luigi', 'Peach', 'Bowser']
    output = ''

    for character in characters:
        output += character

    print output

Our example here doesn't do really interesting things. However, what we can say is in our loop, we create each time a new _String_ object. So, it means that, if our list was more important, we could be confronted with a performance problem.

## Conclusion ##

Through this article we took a look at the concept of _immutability_. We saw what it means, what its benefits are and how we can deal with such a concept.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!