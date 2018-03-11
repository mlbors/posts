# What is a Finite State Machine? #

In this article, we are going to see what a _Finite State Machine_ is.

## Introduction ##

A _Finite State Machine_, or _FSM_, is a computation model that can be used to simulate sequential logic, or, in other words, to represent and control execution flow. _Finite State Machines_ can be used to model problems in many fields, including mathematics, artificial intelligence, games or linguistics. 

## Definition ##

A _Finite State Machine_ is a model of computation based on a hypothetical machine made of one or more states. Only one single state of this machine can be active at the same time. It means the machine has to transition from one state to another in to perform different actions.

A _Finite State Machine_ is any device storing the state of something at a given time. The state will change based on inputs, providing the resulting output for the implemented changes.

_Finite State Machines_ come from a branch of _Computer Science_ called "_automata theory_". The family of data structure belonging to this domain also includes the _Turing Machine_.

The important points here are the following:

* We have a fixed set of states that the machine can be in
* The machine can only be in one state at a time
* A sequence of inputs is sent to the machine
* Every state has a set of transitions and every transition is associated with an input and pointing to a state

## Real world examples ##

Let's see what could be a _Finite State Machine_ in the real world:

### Coin-operated turnstile ###

* **States:** locked, unlocked
* **Transitions:** pointing a coin in the slot will unlock the turnstile, pushing the arm of the unlocked turnstile will let the costumer pass and lock the turnstile again

### Traffic Light ###

* **States:** Red, Yellow, Green
* **Transitions:** After a given time, Red will change to Green, Green to Yellow, and Yellow to Red

### A Safe ###

* **States:** Multiple "locked" states, one "unlocked" state
* **Transitions:** Correct combinations move us from initial locked states to locked states closer to the unlocked state, until we finally get to the unlocked state. Incorrect combinations land us back in the initial locked state

## Programming example ##

Knowing what we know, let's make a simple and basic programming example. Let's say we have our friend Mario in our favourite video game. So, Mario can do the following actions:

* Stand still
* Run
* Jump
* Duck

We can see that Mario can do a lot of things and all of those things should be a specific state. Before writing any single line of code, we should ask ourselves how all of our different states fit together. We need to know exactly what Mario can do and how and when he can do these actions. For example, Mario can stand still then run when we push the corresponding button, but cannot run while he is jumping. So, an input that doesn't cause a change of state is ignored.

We could start first by defining a "_State Interface_":

    interface State
    {
        void enter(Character character);
        State handleInput(Character character, Input input);
        void update(Character character);
    }

Then, we can a create a class for each state:

    class RunningState : State
    {
        void enter(Character character)
        {
            // Various operations like
            // setting the graphics
        }

        State handleInput(Character character, Input input)
        {
            // Various operations like
            // checking the input

            // Returning a state after all the operations
            return new StandingState();
        }

        void update(Character character)
        {
            // Various operations
        }
    }

So, our character, here our dear Mario, could have the following code:

    class Mario : Character, Player
    {
        private State _state;

        void handleInput(Input input)
        {
            _state.handleInput(this, input);
        }

        void update()
        {
            _state.update(this);
        }
    }

## Stack-Based FSM ##

We could go a little further. We could use a _Stack-Based FSM_. With our previous solution, we have no concept of history. We know our current state, but we can't go back to the previous state.

To solve this problem, we could use a _Stack_, which stores elements in _LIFO_ style (_Last In, First Out_), to save our different states. That means the current state is the one on the top of the _Stack_. Then, when we want to transition to a new state, we _push_ that new state onto the _Stack_ and this state becomes the current state. When we are done, we _pop_ this state and the previous state becomes the current state. Of course, it is our responsibility to manage which state we want in the _Stack_ and which we want to discard.

## Conclusion ##

Through this article, we saw what a _Finite State Machine_ is. We saw that a _Finite State Machine_ is a model of computation based on a hypothetical machine made of one or more states and only one single state of this machine can be active at the same time.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
