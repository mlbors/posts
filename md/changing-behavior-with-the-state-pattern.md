# Changing behavior with the State Pattern

In this article, we are going to take a look at the _State Pattern_. Let's get into it!

## Introduction ##

The _State Pattern_ is a _Behavioral Pattern_ in which the behavior of a class changes based on its state. This pattern implements a _state machine_ in an object-oriented way and can be a solution to the problem where an object has to change its behavior at runtime without using large monolithic conditional statements.

## State Machine ##

But what is a _state machine_? Basically, a _state machine_ is a mathematical abstraction that stores the status of something at a given time. It will read a series of external inputs, does some operations and eventually switch to a different state. There are several real-world examples like elevators, whose sequence of stops is determined by the floors requested by riders or traffic lights, which change sequence when cars are waiting.

## The pattern ##

So, with the _State pattern_, we create objects which represent various _States_ and a _Context_ object whose behavior varies as its state object changes.

The _Context_ is class that presents a single interface to the outside world where we will keep a pointer to the current _State_. To change the _State_ of the _state machine_, we will have to change the current _State_ pointer.

We will also create an _abstract class State_ on which every _State_ will be based.

The _State Pattern_ is useful when an object's behavior depends on its state and it must change its behavior at run-time or when we have operations that have multipart conditional statements that depend on the object's state.

This pattern is similar to _Strategy_ but in this last one, the choice of algorithm is more stable. The _State Pattern_ has also the same structure as the _Bridge Pattern_ but they solve different problems: _State_ allows the behavior of an object to change along with its _State_, while _Bridge_ aims to "decouple an abstraction from its implementation so that the two can vary independently".

## Example ##
Now, let's make a very simple example with _PHP_. And because it is always funnier, we are going to say that we are making an awesome video game despite the fact that it is unlikely that we really create a video game with _PHP_.

First, let's define our _interface_ for our _States_:

    interface EnemyState
    {
        public function groan();
    }

Now, we can create a few _States_:

    class NotAngry implements EnemyState
    {
        public function groan(): String
        {
            echo 'Groan';
        }
    }

    class Angry implements EnemyState
    {
        public function groan()
        {
            echo 'Groan louder';
        }
    }

    class VeryAngry implements EnemyState
    {
        public function groan()
        {
            echo 'GROAN LOUDER';
        }
    }

And now we can create our _Context_:

    class Enemy
    {
        protected $state;

        public function __construct(EnemyState $state)
        {
            $this->state = $state;
        }

        public function setState(EnemyState $state)
        {
            $this->state = $state;
        }

        public function groan()
        {
            $this->state->groan();
        }
    }

And it can be used like so:

    $hero = new Hero();
    $enemy = new Enemy(new NotAngry());

    $hero->move();
    $enemy->groan();

    $hero->fire($enemy);
    $enemy->setState(new Angry());
    $enemy->groan();

    $hero->fire($enemy);
    $enemy->setState(new VeryAngry());
    $enemy->groan();

## Conclusion ##

Through this article we saw how works the _State Pattern_ and on which principles it relies. We saw how useful it can be and how to implement it with a really simple example. In brief, the _State Pattern_ allows an object to alter its behavior when its internal state changes. The object will appear to change its class. In this pattern, we define a _Context_, an _abstract State_ and some _concrete States_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!