Simplify everything with the Facade Pattern

In this post, we are going to look at the _Facade Pattern_.

The _Facade Pattern_ is a _structural pattern_ that provides a simplified interface to a complex subsystem. It is like when we turn on our smartphone, we just push one button and we ignore all the operations that have to be done behind.

The goal of the _Facade Pattern_ is to reduce _coupling_ and _complexity_. However, a _Facade_ doesn't forbid us to access to the subsystem. We can also have multiple _Facades_ for one subsystem. 

A _Facade_ can be useful when we want to provide a simple interface to a complex subsystem, to layer our subsystems or when there are many dependencies between clients and the implementation classes of an abstraction.

Let's make a quick example of the _Facade Pattern_ using _PHP_. Let's say we have a spaceship and we want it to take off or land on.

First, we create our _SpaceShip class_.

    class SpaceShip {

        public function getPower() {
            echo "Getting power!";
        }

        public function blink() {
            echo "Blinking lights!";
        }

        public function makeNoise() {
            echo "Beep beep beep!";
        }

        public function leaveTheGround() {
            echo "Leaving the ground!";
        }

        public function getBackOnTheGround() {
            echo "Getting back on the ground";
        }

        public function turnOffEngine() {
            echo "Turning engine off.";
        }

        public function switchOffTheLights() {
            echo "Switching off the lights!";
        }

    }

And now we can create our _Facade_:

    class SpaceShipFacade
    {
        protected $spaceShip;

        public function __construct(SpaceShip $spaceShip)
        {
            $this->spaceShip = $spaceShip;
        }

        public function takeOff()
        {
            $this->spaceShip->getPower();
            $this->spaceShip->blink();
            $this->spaceShip->makeNoise();
            $this->spaceShip->leaveTheGround();
        }

        public function landOn()
        {
            $this->spaceShip->getBackOnTheGround();
            $this->spaceShip->turnOffEngine();
            $this->spaceShip->switchOffTheLights();
        }
    }

And we can use it like so:

    $spaceShip = new SpaceShipFacade(new SpaceShip());
    $spaceShip->takeOff();
    $spaceShip->landOn();