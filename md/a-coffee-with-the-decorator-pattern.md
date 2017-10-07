## [A coffee with the Decorator pattern](https://mlbors.tumblr.com/post/159862673400/a-coffee-with-the-decorator-pattern)

The _Decorator pattern_ is structural pattern that adds new functionality to class instances. Let’s have a coffee with it!

In plain words, the _Decorator pattern_ lets you dynamically change the behavior of an object at run time by wrapping it in an object of a decorator class. _Decorators_ provide a flexible alternative to subclassing for extending functionality.

Let’s make a quick and simple example with coffee and _PHP_!

First of all, we define a _CoffeeInterface_:
    
    
    interface CoffeeInterface
    {
        public function getCost();
        public function getDescription();
    }

And now we create a _SimpleCoffee_ class that implements the _CoffeeInterface_:
    
    
    class SimpleCoffee implements CoffeeInterface
    {
        public function getCost()
        {
            return 10;
        }
    
        public function getDescription()
        {
            return 'Simple coffee';
        }
    }

But what would happen if we took a coffee with milk or with chocolate or with both? If we create a concrete implementation for each, it will be very tedious to manage. And how could we handle a thousand of different kinds of coffee? That is where we can use the _Decorator pattern_! With that pattern, we can easily manage a coffee with milk and chocolate or with something else.

So, we have a coffee with milk:
    
    
    class MilkCoffee implements CoffeeInterface
    {
        protected $coffee;
    
        public function __construct(CoffeeInterface $coffee)
        {
            $this->coffee = $coffee;
        }
    
        public function getCost()
        {
            return $this->coffee->getCost() + 2;
        }
    
        public function getDescription()
        {
            return $this->coffee->getDescription() . ', milk';
        }
    }

…and with chocolate: 
    
    
    class ChocolateCoffee implements CoffeeInterface
    {
        protected $coffee;
    
        public function __construct(CoffeeInterface $coffee)
        {
            $this->coffee = $coffee;
        }
    
        public function getCost()
        {
            return $this->coffee->getCost() + 4;
        }
    
        public function getDescription()
        {
            return $this->coffee->getDescription() . ', chocolate';
        }
    }

And we use them like so:
    
    
    //A simple coffee
    $someCoffee = new SimpleCoffee();
    echo $someCoffee->getCost();
    echo $someCoffee->getDescription();
    
    //A coffee with milk
    $someCoffee = new MilkCoffee($someCoffee);
    echo $someCoffee->getCost();
    echo $someCoffee->getDescription();
    
    //A coffee with milk and chocolate
    $someCoffee = new ChocolateCoffee($someCoffee);
    echo $someCoffee->getCost();
    echo $someCoffee->getDescription();
