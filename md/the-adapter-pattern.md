# The Adapter Pattern #

In this post, we are going to see what is the _Adapter pattern_ and how we can use it. Let's get into it!

The _Adapter pattern_ is a _Structural pattern_. This kind of pattern is related the relationships between entities.

_The Adapter pattern_ allows us to make classes work together that otherwise couldn't because of incompatible interfaces. Thus, the classes can work with each other without us having to modify their source code.

A very straightforward example in real life is the power adapter: a three-pin plug can't be connected to a two-prong outlet.

This pattern can be really useful when we want to use an existing class and its interface doesn't match the one we need.

Now let's have a quick example and because it is always more fun, we are going to say that we are building a video game (and for the sake of this example we are going to do it with _PHP_).

In our brand new game we have a space hunter which is navigating through the entire galaxy killing dangerous aliens with his bare hands. Now for some reasons, that same guy needs to fight with a dragon. The problem is that the dragon interface is different from the one used with aliens. So we can use the _Adapter pattern_ to make it works.

First, we create our _SpaceHunter_ class that expects any implementation of _AlienInterface_ to kill:

    class SpaceHunter
    {
        public function kill(Alien $alien)
        {
            $this->killTheAlienWithBareHands();
        }
    }

Now, let's define our _AlienInterface_ and different types of aliens:

    interface Alien
    {
        public function fireLaser();
    }

    class AlienFromJupiter implements Alien
    {
        public function fireLaser()
        {
            $this->doSomething();
        }
    }

    class AlienFromBetelgeuse implements Alien
    {
        public function fireLaser()
        {
            $this->doSomething();
        }
    }

Then, we can define our _DragonInterface_ and a concrete implementation:

    interface Dragon 
    {
        public function breatheFire();
    }

    class GalacticDragon implements Dragon
    {
        public function breatheFire()
        {
            $this->doSomething();
        }

    }

And now we can create our _Adapter_:

    class DragonAdapter implements Alien
    {
        protected $dragon;

        public function __construct(Dragon $dragon)
        {
            $this->dragon = $dragon;
        }

        public function fireLaser()
        {
            $this->dragon->breatheFire();
        }
    }

And we can use it like so:

    $galacticDragon = new GalacticDragon();
    $dragonAdapter = new DragonAdapter($galacticDragon);

    $spaceHunter = new SpaceHunter();
    $spaceHunter->kill($dragonAdapter);