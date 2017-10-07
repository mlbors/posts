# What is Big O notation?

This post describes the real basics of Big O notation and doesn’t cover the entire subject. It is just a simple and brief introduction.

Big O notation is the mathematical notation used to express the performance or complexity of an algorithm. Big O notation specifically describes the worst-case scenario, and can be used to describe the execution time required or the space used by an algorithm. 

If you have something like O(1), it describes an algorithm that will always execute in the same time, for example, print a specific element of an array. 
    
    
    public function foo($myArray) {
      echo $myArray[0];
    } 

O(N) describes an algorithm whose performance will grow linearly and in direct proportion to the size of the input data set, for example, print all the elements of an array. 
    
    
    public function foo($myArray) {
      foreach($myArray as $item) {
        echo $item;
      } 
    }

But if there is a loop inside the parent loop, it will be noted O(n^2), because it is a quadratic function. 
    
    
    public function foo($myArray) {
      foreach($myArray as $item) {
        foreach($item as $subitem) {
          echo $subitem;
        }
      }
    } 

These are the real basics of Big O notation and there are more complex things about it. Maybe I will continue in a further post. 
