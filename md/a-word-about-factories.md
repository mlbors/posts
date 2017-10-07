## [A word about Factories](https://mlbors.tumblr.com/post/160872455020/a-word-about-factories)

Through this article, we are going to overview the _Factory_ pattern that belongs to _Creational design patterns_.

_Creational patterns_ offer ways to instantiate an object or group of related objects. They deal with object creation mechanisms, trying to create objects in a manner suitable to the situation.

Here, we are going to talk about three different kinds of factories: _Simple Factory_, _Factory Method_ and _Abstract Factory_. Our examples will be written in _PHP_.

## Simple Factory

_Simple factory simply_ generates an instance for client without exposing any instantiation logic to the client. In other words, a factory is a function or method that returns objects from some method call, which is assumed to be “new”.

We can use this pattern when creating an object is not just a few assignments and involves some logic.

Let’s make a simple example with houses. First of all we have a house interface and its implementation:
    
    
    interface HouseInterface
    {
        public function getWidth(): float;
        public function getHeight(): float;
    }
    
    class WoodenHouse implements HouseInterface
    {
        protected $width;
        protected $height;
    
        public function __construct(float $width, float $height)
        {
            $this->width = $width;
            $this->height = $height;
        }
    
        public function getWidth(): float
        {
            return $this->width;
        }
    
        public function getHeight(): float
        {
            return $this->height;
        }
    }

Now, we can make our house factory that makes houses and returns them:
    
    
    class HouseFactory
    {
        public static function makeHouse($width, $height): HouseInterface
        {
            return new WoodenHouse($width, $height);
        }
    }

We can use it like so:
    
    
    $house = HouseFactory::makeHouse(100, 200);
    echo $house->getWidth() . '<br />';
    echo $house->getHeight() . '<br />';

## Factory Method

This pattern provides a way to delegate the instantiation logic to child classes. The factory method pattern deals with the problem of creating objects without having to specify the exact class of the object that will be created.

The _Factory Method_ is useful when the client doesn’t know what exact sub-class it might need.

Now, for example, let’s say we have a music band where every member plays a different instrument. We firstly create a musician interface and two concrete implementations:
    
    
    interface MusicianInterface
    {
        public function plays(): String;
    }
    
    class Guitarist implements MusicianInterface
    {
        public function plays(): String
        {
            return 'Playing guitar.';
        }
    }
    
    class Pianist implements MusicianInterface
    {
        public function plays(): String
        {
            return 'Playing piano';
        }
    }

Now we define an abstract class for the band members:
    
    
    abstract class BandMember
    {
        abstract public function makeMusician(): MusicianInterface;
    
        public function makeTheShow(): String
        {
            $musician = $this->makeMusician();
            return $musician->plays();
        }
    }

The above _abtract class_ will delegate te creation of a band member to every class that extends it:
    
    
    class GuitaristMember extends BandMember
    {
        public function makeMusician(): MusicianInterface
        {
            return new Guitarist();
        }
    }
    
    class PianistMember extends BandMember
    {
        public function makeMusician(): MusicianInterface
        {
            return new Pianist();
        }
    }

We can use it like this:
    
    
    $guitaristMember = new GuitaristMember();
    echo $guitaristMember->makeTheShow() . '<br />';
    
    $pianistMember= new PianistMember();
    echo $pianistMember->makeTheShow() . '<br />';

## Abstract Factory

The _Abstract Factory pattern_ is a factory of factory. It groups the individual but related factories together without specifying their concrete classes.

We can use this pattern when there are interrelated dependencies with complex creation logic involved.

Let’s imagine an example with trains. For the sake of this example, we are going to assume that we have different kinds of trains and each kind has a specific kind of pilot.

First, we define a interface for trains and another one for pilots.
    
    
    interface TrainInterface
    {
        public function getDescription(): String;
    }
    
    interface PilotInterface
    {
        public function getDescription(): String;
    }

Now we can define another interface for the train factory:
    
    
    interface TrainFactoryInterface
    {
        public function makeTrain(): TrainInterface;
        public function makePilot(): PilotInterface;
    }

Now we can create our different factories: 
    
    
    class ToyTrainFactory implements TrainFactoryInterface
    {
        public function makeTrain(): TrainInterface
        {
            return new ToyTrain();
        }
    
        public function makePilot(): PilotInterface
        {
            return new ToyTrainPilot();
        }
    }
    
    class VaporTrainFactory implements TrainFactoryInterface
    {
        public function makeTrain(): TrainInterface
        {
            return new VaporTrain();
        }
    
        public function makePilot(): PilotInterface
        {
            return new VaporTrainPilot();
        }
    }

Now we have to define our trains and our pilots: 
    
    
    class ToyTrain implements TrainInterface
    {
        public function getDescription(): String
        {
            return 'I am a toy train.';
        }
    }
    
    class ToyTrainPilot implements PilotInterface
    {
        public function getDescription(): String
        {
            return 'I am a toy trains pilot.';
        }
    }
    
    class VaporTrain implements TrainInterface
    {
        public function getDescription(): String
        {
            return 'I am a vapor train.';
        }
    }
    
    class VaporTrainPilot implements PilotInterface
    {
        public function getDescription(): String
        {
            return 'I am a vapor trains pilot.';
        }
    }

We can use it like so:
    
    
    $vaporTrainFactory = new VaporTrainFactory();
    $vaporTrain = $vaporTrainFactory->makeTrain();
    $vaporTrainPilot = $vaporTrainFactory->makePilot();
    
    echo $vaporTrain->getDescription() . '<br />';
    echo $vaporTrainPilot->getDescription() . '<br />';
    
    $toyTrainFactory = new ToyTrainFactory();
    $toyTrain = $toyTrainFactory->makeTrain();
    $toyTrainPilot = $toyTrainFactory->makePilot();
    
    echo $toyTrain->getDescription() . '<br />';
    echo $toyTrainPilot->getDescription() . '<br />';
