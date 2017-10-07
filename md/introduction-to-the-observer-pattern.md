## [Introduction to the Observer pattern](https://mlbors.tumblr.com/post/159600546460/introduction-to-the-observer-pattern)

The _Observer pattern_ provides a way to notify objects when another object has changed. Let’s dive into it!

The _Observer pattern_ defines a one-to-many dependency between objects so that whenever an object changes its state, all its dependents are notified and updated automatically.

The main object is called the “Subject” and its dependents are called the “Observers”. Usually, the _Subject_ notifies the _Observers_ by calling one of their methods. Let’s make a quick example with _PHP_.

First, we define an _Observer interface_ that will contain the _update_ method.
    
    
    interface ObserverInterface
    {
        public function update($subject);
    }

Now, we do the same thing for the _Subject_, so every object that will implement this interface will be able to _attach_, _detach_ or _notify_ their _Observers_.
    
    
    interface SubjectInterface
    {
        public function attach($observer);
    
        public function detach($observer);
    
        public function notify();
    }

Then we write a concrete implementation of the _Subject interface_.
    
    
    class User implements SubjectInterface
    {
        private $name;
        private $observers = array();
    
        public function __construct($name)
        {
            $this->name = $name;
        }
    
        public function getName()
        {
            return $this->name;
        }
    
        public function attach($observer)
        {
            $this->observers[] = $observer;
        }
    
        public function detach($observer)
        {
            $key = array_search($observer, $this->observers, true);
    
            if ($key) {
                unset($this->observers[$key]);
            }
        }
    
        public function initUser()
        {
            $this->notify();
        }
    
        public function notify()
        {
            foreach ($this->observers as $value) {
                $value->update($this);
            }
        }
    }

And now we implement the _Observer interface_.
    
    
    class UserObserver implements ObserverInterface
    {
        private $userCount = 0;
    
        public function __construct()
        {
        }
        
        public function update($user)
        {
            if (!empty($user)) {
                echo "User " . $user->getName() . " was initiated.";
    
                $this->userCount++;
            }
        }
    
        public function getUserCount()
        {
            return $this->userCount;
        }
    }

Let’s try what we made!
    
    
    $observer = new UserObserver();
    $user = new User("John Doe");
    $user->attach($observer);
    
    $user->initUser();
    echo $observer->getUserCount();
