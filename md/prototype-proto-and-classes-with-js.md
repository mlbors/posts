## [prototype, __proto__ and classes with JS](https://mlbors.tumblr.com/post/159638107370/prototype-proto-and-classes-with-js)

Those aforementioned three words are relative to _JavaScript_ and could sound very weird. Let’s try to understand them!

_JavaScript’s_ object system is based on _prototypes_, not _classes_. But what is the difference?

A _class_ is like a blueprint. It is a description of the object to be created. _Classes_ inherit from classes and create subclass relationships.

A _prototype_ is a working object instance. Objects inherit directly from other objects.

Every _JavaScript_ object has a _prototype_. The _prototype_ is also an object. All _JavaScript_ objects inherit their properties and methods from their _prototype_. Objects created using an object literal, or with _new Object()_, inherit from a _prototype_ called _Object.prototype_. The _Object.prototype_ is on the top of the _prototype chain_.
    
    
    function talk() {
      console.log(this.sound);
    }
    
    let animal() {
      talk;
    }
    
    let cat() {
      sound: "Dog"
    }
    
    let cat() {
      sound: "Maouw"
    }
    
    Object.setPrototypeOf(dog, animal);
    Object.setPrototypeOf(cat, animal);
    
    dog.talk; //will output "Woof"
    cat.talk; //will output "Maouw"

If we want to add a property or a method to the _prototype_, we can do it like so:
    
    
    Animal.prototype.name = "My pet"; 
    Animal.prototype.beNice = function() {
        console.log("I am nice");
    };

We can also use the _new_ operator which will call a _constructor_ function. A _constructor_ could be any function. Basically, when a _constructor_ function is called, the following things happen:

  * A new object is created
  * The _constructor_ property of the object is set
  * The object is set to delegate to the _prototype_
  * The _prototype_ is called in the context of the new object



So, with a _constructor_ function, we can use the _new_ keyword to create new objects from the same _prototype_:
    
    
    function Animal(sound) {
      this.sound = sound;
    
      this.makeSound = function() {
        console.log(this.sound);
      }
    }
    
    var dog = new Animal("Woof");
    var cat = new Animal("Maouw"); 
    
    dog.makeSound(); //will output "Woof"
    cat.makeSound(); //will output "Maouw"

Adding a new property to an existing object can be done like that:
    
    
    dog.color = "Brown";

The property will be added to _dog_, not to _cat_ and not to any other _Animal objects_.

Adding a new method to an existing object can be done this way:
    
    
    cat.playWithMouse = function () {
        return "I am playing with a mouse!";
    };

The method will only be added to _cat_.

Now, maybe you saw something like ___proto___ and asked what it is.

___proto___ is the actual object that is used in the lookup chain to resolve methods. It is a property that all objects have. This is the property which is used by the _JavaScript_ engine for inheritance. According to _ECMA specifications_ it is supposed to be an internal property, however most vendors allow it to be accessed and modified. The use of ___proto___ is controversial, and has been discouraged. It was never originally included in the _ECMA specifications_, but modern browsers decided to implement it anyway.

_prototype_ is a property belonging only to functions. It is used to build ___proto___ when the function happens to be used as a _constructor_ with the _new_ keyword.

So, where are the _classes_? In essence, a _JavaScript class_ is just a Function object that serves as a _constructor_ plus an attached _prototype_ object Empty also referred as this. _Classes_ in _ES6_ don’t add any functionality to what we already have in the language, they are just a simpler syntax for building the same objects as we had before. The _class_ syntax is not introducing a new object-oriented inheritance model to _JavaScript_. _JavaScript classes_ provide a much simpler and clearer syntax to create objects and deal with inheritance.
    
    
    class Animal {
        constructor(sound) {
            this.sound = sound;
        }
    
        talk() {
            console.log(this.sound);
        }
    }
