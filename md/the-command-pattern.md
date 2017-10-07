## [The Command pattern](https://mlbors.tumblr.com/post/161164215695/the-command-pattern)

In this post we are going to go through the _Command pattern_.

The _Command pattern_ is a _Behavioral pattern_ that allows us to encapsulate actions in objects. The main idea behind this pattern is to decouple client from receiver. The encapsulated information lets us trigger an event or an action at a later time and includes the method name, the object that owns method and values for the method parameters.

A classic example would be us (i.e. Client) switching on (i.e. Command) the television (i.e. Receiver) using a remote control (Invoker).

The Command pattern can be used when we want to parameterize objects by an action to perform, specify, queue and execute requests at different times or support undo (non exhaustive list).

Let’s make an example with a television and a remote control:

First, we create an implementation for the _Receiver_ (television):
    
    
    class Television
    {
        public function turnOn()
        {
            echo "TV is on!";
        }
    
        public function turnOff()
        {
            echo "TV is off!";
        }
    }

The we define a _Command interface:_
    
    
    interface CommandInterface
    {
        public function execute();
        public function undo();
        public function redo();
    }
    

Now we can write down a set of commands:
    
    
    class TurnOnCommand implements CommandInterface
    {
        protected $television;
    
        public function __construct(Television $television)
        {
            $this->television = $television;
        }
    
        public function execute()
        {
            $this->television->turnOn();
        }
    
        public function undo()
        {
            $this->television->turnOff();
        }
    
        public function redo()
        {
            $this->execute();
        }
    }
    
    class TurnOffCommand implements CommandInterface
    {
        protected $television;
    
        public function __construct(Television $television)
        {
            $this->television = $television;
        }
    
        public function execute()
        {
            $this->television->turnOff();
        }
    
        public function undo()
        {
            $this->television->turnOn();
        }
    
        public function redo()
        {
            $this->execute();
        }
    }

Finally we can create our _Invoker_ (remote control):
    
    
    class RemoteControl
    {
        public function submit(CommandInterface $command)
        {
            $command->execute();
        }
    }

We can use it like so:
    
    
    $television = new Television();
    
    $turnOn = new TurnOnCommand($television);
    $turnOff = new TurnOffCommand($television);
    
    $remote = new RemoteControl();
    $remote->submit($turnOn);
    $remote->submit($turnOff);
