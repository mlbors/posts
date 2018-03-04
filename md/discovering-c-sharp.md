# Discovering C# #

Through this article, we are going to take a look at the programming language named _C#_. Let's get into it!

## Introduction ##

First of all, we are not going to look at _C#_ in every single detail because it is not the purpose of this article and it would probably take a whole book. Our goal here is to simply get familiar with this very common language. We are going to have a look at the very basics of this language and go a little further after that. This article is some kind of introduction to this language.

## What is C#? ##

_C#_, or _C Sharp_, is a programming language developed by _Microsoft_. We usually say that it is an object-oriented programming language, but in fact, it is a multi-paradigm programming language. It was designed to be used with the _.NET Framework_. It is derived from _C_ and _C++_ and it is really similar to _Java_.

_C#_ programs run on the _.NET Framework_ through a virtual execution system called the _Common Language Runtime_ (_CLR_). When we compile a program written in _C#_, it is transformed to an _Intermediate Language_ (_IL_). That last code and all the resources a stored in an executable file called Assembly. When the program is executed, the _Assembly_ is loaded into the _CLR_ that will perform the _Just-in-Time compilation_ (_JIT_) that will convert the _Intermediate Language_ code to native machine instructions. In a very few words, we can say that the _CLR_ is to _.NET_ the same thing as the _JVM_ is to _Java_.

## Generalities and syntax ##

A source code written in _C#_ is placed in a file that has ".cs" as extension. Lines written in this file have to be read from left to right, from top to bottom. Every instruction should be ended with a semicolon (;).

_C#_ is case-sensitive.

We can add comments to our code using "//" for a single line, or "/*" and "*/" for multiple lines.

## Variables ##

A variable is like a box where we store a value. It is a name given to a data value. The content of a variable can vary at any time. In _C#_, a variable has to be defined with a data type. A variable can be declared and initialized later or it can be declared and initialized at the same time.

    // Declaring a variable
    string message;
    // Assigning a value the previously declared variable
    message = "Hello World!!";

    // Declaring and initializing a variable
    string message = "Hello World!!";

## Data Types ##

Because _C#_ is a strongly typed language, we are required to inform the compiler about which data type we want to use with every variable we declare. A data type specifies the type of data that a variable can store.

The information stored in a type can include the following:

* The storage space that a variable of the type requires
* The maximum and minimum values that it can represent
* The members (methods, fields, events, and so on) that it contains
* The base type it inherits from
* The location where the memory for variables will be allocated at run time
* The kinds of operations that are permitted

The compiler uses type information to make sure that all operations that are performed in our code are type safe. 

    // Declaring a string
    string stringVar = "Hello World!!";
    // Declaring a integer
    int intVar = 100;
    // Declaring a float
    float floatVar = 10.2f;
    // Declaring a character
    char charVar = 'A';
    // Declaring a boolean
    bool boolVar = true;

The following table lists the data types available in _C#_ along with the range of possible values for each data type: 

|Alias|.NET Type|Type|Size (bits)|Range (values)|
|--- |--- |--- |--- |--- |
|byte|Byte|Unsigned integer|8|0 to 255|
|sbyte|SByte|Signed integer|8|-128 to 127|
|int|Int32|Signed integer|32|-2,147,483,648 to 2,147,483,647|
|uint|UInt32|Unsigned integer|32|0 to 4294967295|
|short|Int16|Signed integer|16|-32,768 to 32,767|
|ushort|UInt16|Unsigned integer|16|0 to 65,535|
|long|Int64|Signed integer|64|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807|
|ulong|UInt64|Unsigned integer|64|0 to 18,446,744,073,709,551,615|
|float|Single|Single-precision floating point type|32|-3.402823e38 to 3.402823e38|
|double|Double|Double-precision floating point type|64|-1.79769313486232e308 to 1.79769313486232e308|
|char|Char|A single Unicode character|16|Unicode symbols used in text|
|bool|Boolean|Logical Boolean type|8|True or False|
|object|Object|Base type of all other types|
|string|String|A sequence of characters|
|decimal|Decimal|Precise fractional or integral type that can represent decimal numbers with 29 significant digits|128|(+ or -)1.0 x 10e-28 to 7.9 x 10e28|
|DateTime|DateTime|Represents date and time||0:00:00am 1/1/01 to 11:59:59pm 12/31/9999|

## Value Type and Reference Type ##

In _C#_, data types are categorized based on how they store their value in the memory. They could be a value type or a reference type.

### Value Type ###

A data type is a value type if it holds a data value within its own memory space. It means variables of these data types directly contain their values.

The following data types are all of value type:

* bool
* byte
* char
* decimal
* double
* enum
* float
* int
* long
* sbyte
* short
* struct
* uint
* ulong
* ushort

When we pass a value type from one method to another, the system creates a separate copy of that variable in the other method. So if the value is changed in one method, the value in the other method won't be affected.

### Reference Type ###

A data type is a reference type if it stores the memory address where the value is being stored. In other words, a reference type contains a pointer to another memory location that holds the data. 

The following data types are of reference type:

* String
* All arrays, even if their elements are value types
* Class
* Delegates

When we pass a reference type from one method to another, the system passes the address of the variable. It means that if the value is changed in one method, the value in the other method will also be affected.

## Operators ##

An operator is a symbol that is used to perform operations. Some operators have different meanings based on the data type of the operand.

The following table lists some of the operators available in _C#_:

|Operator category|Operators|
|--- |--- |
|Primary|x.y|
|Unary|+x|
|Multiplicative|x * y|
|Additive|x + y|
|Shift|x << y|
|Relational and type testing|x < y|
|Equality|x == y|
|Logical AND|x & y|
|Logical XOR|x ^ y|
|Logical OR|x | y|
|Conditional AND|x && y|
|Conditional OR|x \|\| y|
|Null-coalescing|x ?? y|
|Conditional|?:|
|Assignment and lambda expression|x = y|

## Condition ##

In _C#_ programming, there are various types of decision-making statements:

* if statement
* if-else statement
* Nested if statement
* if-else-if statement
* switch statement

### If statements ###

The _if_ statement contains a boolean expression inside brackets followed by a single or multi-line code block. At runtime, if a boolean expression is evaluated to true, then the code block will be executed.

    // if statement
    if (a > b)
    {
        Console.WriteLine("a is greater than b");
    }

    // if-else statement
    if (a > b)
    {
        Console.WriteLine("a is greater than b");
    }
    else
    {
        Console.WriteLine("a is either equal to or less than b");
    }

    // if-else-if statement
    if (a > b)
    {
        Console.WriteLine("a is greater than b");
    }
    else if (a < b)
    {
        Console.WriteLine("a is less than b");
    }
    else
    {
        Console.WriteLine("a is equal ton b");
    }

    // Nested if statement
    if (a > 0)
    {
        if (a <= 100)
        {
            Console.WriteLine("a is positive number less than 100");
        }
        else 
        {
            Console.WriteLine("a is positive number greater than 100");
        }
                    
    }

### Switch ###

The _switch_ statement executes a code block depending upon the resulted value of an expression. It is like the if-else-if statement.

    switch (a)  
    {  
        case 10: 
            Console.WriteLine("It is 10");
        break;  
        case 20: 
            Console.WriteLine("It is 20"); 
        break;  
        case 30: 
            Console.WriteLine("It is 30"); 
        break;  
        default: 
            Console.WriteLine("Not 10, 20 or 30"); 
        break;  
    }

## Loops ##

A loop gives us the ability to repeat a block of code. In _C#_, there are four ways to achieve a loop.

### While loop ###

A _While loop_ is used to iterate a part of the program while a condition is true.

    int i = 0;

    while (i < 10)
    {
        Console.WriteLine("Value of i: {0}", i);
        i++;
    }

### Do-While loop ###

A _Do-While loop_ is like a While loop, except that the block of code will be executed at least once because the loop executes the block of code first and then checks the condition. 

    int i = 0;

    do
    {
        Console.WriteLine("Value of i: {0}", i);
        i++;
    } while (i < 10);

### For loop ###

A _For loop_ executes a block of statements repeatedly until the specified condition returns false. 

    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine("Value of i: {0}", i);
    }

### Foreach loop ###

A _Foreach_ statement provides a way to iterate through the elements of an array or any enumerable collection. 

    int[] numbers = { 4, 5, 6, 1, 2, 3, -2, -1, 0 };

    foreach (int i in numbers)
    {
        System.Console.Write("{0} ", i);
    }

## Arrays ##

In _C#_, an array is a group of similar types of elements that have contiguous memory location. An array is a special type of data type which can store a fixed number of values sequentially using special syntax. Array index starts from 0.

Like a variable, an array can be declared and initialized later or it can be declared and initialized at the same time.

    // Declaring an array that contains strings
    string[] names;
    // Instantiating the array and defining its size
    string[] names = new string[2];
    // Storing a value at index 0
    names[0] = "John Doe";
    // Displaying the value stored at index 0
    Console.WriteLine(intArray[0]);   

## Collections ##

In _C#_, a collection represents a group of objects. Unlike an array, a collection doesn't have a fixed size. There are several types of collections.

### ArrayList ###

An _ArrayList_ stores objects of any type like an array. 

    // Declaring ArrayList
    ArrayList arrayList = new ArrayList();
    // Adding elements
    arrayList.Add(1);
    arrayList.Add("Two");
    // Add an element at a specific index
    arrayList.Insert(1, "Second Item");
    // Removing element at a specific index
    arrayList.RemoveAt(1);

### SortedList ###

A _SortedList_ stores key and value pairs. It automatically arranges elements in ascending order of key by default. 

    // Declaring SortedList
    SortedList sortedList = new SortedList();
    // Adding elements
    sortedList.Add(3, "Three");
    sortedList.Add(4, "Four");
    sortedList.Add(1, "One");
    sortedList.Add(5, "Five");
    sortedList.Add(2, "Two");

### Stack ###

A _Stack_ stores the values in _LIFO_ style (_Last In First Out_). It provides a _Push()_ method to add a value and _Pop()_ and _Peek()_ methods to retrieve values. 

    // Declaring Stack
    Stack stack = new Stack();
    // Adding elements
    stack.Push("John Doe");
    stack.Push(1);
    stack.Push(2);
    stack.Push(null);
    stack.Push(3);
    // Displaying the top item from the stack
    Console.WriteLine(stack.Peek());
    // Removing and returning the item from the top of the Stack
    stack.Pop()

### Queue ###

A _Queue_ stores the values in _FIFO_ style (_First In First Out_). It keeps the order in which the values were added. It provides an _Enqueue()_ method to add values and a _Dequeue()_ method to retrieve values from the collection.

    // Declaring Queue
    Queue queue = new Queue();
    // Adding elements
    queue.Enqueue(3);
    queue.Enqueue(2);
    queue.Enqueue(1);
    // Displaying the first item of the Queue
    Console.WriteLine(queue.Peek());
    // Removing and returning the item from the beginning of the queue
    queue.Dequeue();

### HashTable ###

A _HashTable_ stores key and value pairs. It retrieves the values by comparing the hash value of the keys. 

    // Declaring Hashtable
    Hashtable hashtable = new Hashtable();
    // Adding elements
    hashtable.Add(1, "One");
    hashtable.Add(2, "Two");
    hashtable.Add(3, "Three");
    hashtable.Add("Fr", "Four");
    // Accessing element
    string str = (string)hashtable[2];
    // Removing element
    hashtable.Remove(3);

## Tuples ##

A tuple is an ordered immutable sequence, fixed-size of heterogeneous objects. Tuples allow to return multiple values from a method.

    // Declaring a tuple
    var numbers = ("One", "Two", "Three", "Four", "Five");

    // Declaring another tuple
    (string, string, int) person = ("John", "Doe", 30);

    // Declaring another tuple and accessing values.
    var person = (firstName: "John", lastName: "Doe", Age: 30);
    person.firstName;

    We can also use a tuple as a return type.

    (int Val1, int Val2) Values()
    {
        int val1 = 1;
        int val2 = 2;
        return (val1, val2);
    }

    var values = GetValues();

## Classes ##

A class is like a blueprint. It is a template from which objects are created. In an object-oriented programming language like _C#_, an object is like in the real world: it has properties and functionalities. So, a class defines the kinds of data and the functionalities that an object will have.

### Access modifiers ###

Access modifiers are applied to the declaration of the class, methods, properties, fields and other members. They define the accessibility of the class and its members. There are four access modifiers:

* public - allows a class to expose its member variables and member functions to other functions and objects
* private - allows a class to hide its member variables and member functions from other functions and objects
* protected - allows a child class to access the member variables and member functions of its base class
* internal - allows a class to expose its member variables and member functions to other functions and objects in the current assembly

### Fields ###

A field is a class level variable that can hold a value.

### Properties ###

A property allows us to control the accessibility of a class variable. It encapsulates a private field and provides a level of abstraction allowing us to change a field while not affecting the external way they are accessed by the things that use our class.

### Constructor ###

A class constructor is a special member function that is executed whenever a new object of that class is created. A constructor has exactly the same name as the class and it does not have any return type.

### Methods ###

A method is a group of statements that perform a task. A method is basically written like so:

    <Access Modifier <Return Type> <Method Name>(Parameter List) {
        Method Body
    }

### Namespaces ###

A namespace is a container for a set of related classes. A class name declared in one namespace does not conflict with the same class name declared in another.

### Example ###

Let's put what we saw together to create a simple class:

    // Namespace
    namespace MyNamespace
    {
        // Declaring the class - access modifier, class keyword, class name
        public class MyClass
        {
            // Fields - access modifier, type, field name
            public string field = string.Empty;

            // Property
            private int propertyVar;
            
            // Property accessors
            public int Property
            {
                get { return propertyVar; }
                set { propertyVar = value; }
            } 

            // Constructor
            public MyClass()
            {
            }

            // Method - acces modifier, return type, method name, parameters
            public void MyMethod(int parameter1, string parameter2)
            {
                Console.WriteLine("First Parameter {0}, second parameter {1}", parameter1, parameter2);
            }

            // Method - acces modifier, return type, method name, parameters
            public int MyMethod(int parameter1, int parameter2)
            {
                return parameter1 + parameter2;
            }
        }
    }

## Interfaces ##

An interface is like a contract. Every class that inherits from a specific interface has to implement what is defined in the interface. An interface only contains the declaration of the methods and the properties that a class has to implement.

    // Declaring the interface
    interface MyInterface
    {
        void MyMethod(string message);
    }

    // Implementing the interface
    class MyClass : MyInterface
    {
        public void MyMethod(string message)
        {
            Console.WriteLine(message);
        }
    }

An interface can also contain properties.

    interface MyInterface
    {
        string Property
        {
            get;
            set;
        }

        void MyMethod(string message);
    }

    class MyClass : MyInterface
    {
        private string property;
        public string Property
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }

        public void MyMethod(string message)
        {
            Console.WriteLine(message);
        }
    }

## Structs ##

A struct is mostly like a class, but while a class is a reference type, a struct is a value type data type. Unlike a class, it does not support inheritance and can't have a default constructor.

We can consider defining a struct instead of a class if instances of the type are small and commonly short-lived or are commonly embedded in other objects. The general rule to follow is that structs should be small, simple collections of related properties, that are immutable once created.

    struct Point {
        private int x, y;

            public int XPoint {
                get 
                {
                    return x;
                }
                set 
                {
                    x = value;
                }
            }

            public int YPoint
            {
                get
                {
                    return y;
                }
                set
                {
                    y = value;
                }
            }
        
        public Point(int p1, int p2)
        {
            x = p1;
            y = p2;
        }
    }  

## Enums ##

An enum is a value type data type. It is used to declare a list of named integral constants that may be assigned to a variable. An enum is used to give a name to each constant so that the constant integer can be referred using its name. 

    enum Days
    {
        Sunday,
        Monday,
        Tuesday,
        Wednesday,
        Thursday,
        Friday, 
        Saturday
    }

    Console.WriteLine(Days.Tuesday);
    Console.WriteLine((int)Days.Friday);

## Delegates ##

In _C#_, a delegate is like a pointer to a function. It is a reference type data type and it holds the reference to a method. When we instantiate a delegate, we can associate its instance with any method with a compatible signature and return type. Delegates are used for implementing events and callback methods.

    // Declaring the delegate
    delegate int Calculator(int n);

    class Program
    {   
        static int number = 1;  

        public static int add(int n)  
        {  
            number = number + n;  
            return number;  
        }  

        public static int mul(int n)  
        {  
            number = number * n;  
            return number;  
        }  

        public static int getNumber()  
        {  
            return number;  
        }  

        public static void Main(string[] args)  
        {  
            // Instantiating the delegate  
            Calculator c1 = new Calculator(add);
            // Instantiating the delegate  
            Calculator c2 = new Calculator(mul);  

            // Calling method using delegate  
            c1(20);
            // Calling method using delegate  
            c2(3);  
    
        } 
    }

## Anonymous functions and Lambdas ##

An anonymous function is a type of function that doesn't have a name.

In _C#_, there are two types of anonymous functions:

* Lambda expressions
* Anonymous methods

### Lambdas Expressions ###

A lambda expression is an anonymous function that we can use to create delegates. We can use a lambda expression to create a local function that can be passed as an argument.

    class Program  
    {  
        delegate int Square(int num);  
        static void Main(string[] args)  
        {  
            Square getSquare = x => x * x;  
            int a = getSquare(5);    
            Console.WriteLine(a);  
        }  
    }  

### Anonymous Methods ###

An anonymous method is quite like a lambda expression but allows us to omit the parameter list.

    class Program  
    {  
        public delegate void AnonymousFunction();  
        static void Main(string[] args)  
        {  
            AnonymousFunction myFunction = delegate() {  
                Console.WriteLine("This is an anonymous function");  
            };  
            myFunction();  
        }  
    }

## Generics ##

Generics allow us to define a class with placeholders for the type of its fields, methods or parameters. The compiler will replace these placeholders with the specified type at compile time. Generics increase code reusability.

    // Declaring a generic class
    class GenericClass<T>
    {
        // Declaring a generic field
        private T genericField;

        // Declaring a generifc property
        public T genericProperty { get; set; }

        // Constructor
        public GenericClass(T value)
        {
            genericField = value;
        }

        // Declaring a generic method
        public T genericMethod(T genericParameter)
        {
            Console.WriteLine("Parameter type: {0}, value: {1}", typeof(T).ToString(),genericParameter);
            Console.WriteLine("Return type: {0}, value: {1}", typeof(T).ToString(), genericField);
            return genericField;
        }
    }

    class Program  
    {  
        static void Main(string[] args)  
        {  
            // Using the generic class
            GenericClass<int> genericClass = new GenericClass<int>(10);
        }  
    }

We can use constraints to specify which type of placeholder is allowed with a generic class.

    // Declaring a generic class specifying that T must be a reference type.
    class GenericClass<T> where T: class
    {
        ...
    }

## Generic collections ##

We mentioned collections before. In _C#_, we can also have generic collections.

* List<T> - contains elements of specified type
* Dictionary<TKey, TValue> - contains key-value pairs
* SortedList<TKey, TValue> - stores key and value pairs in an ordered manner
* Hashset<T> - contains non-duplicate elements
* Queue<T> - a Queue containing elements of specified type
* Stack<T> - a Stack containing elements of specified type

## Polymorphism ##

In _C#_, polymorphism can be used in different manners. There are two types of polymorphism in _C#_: compile time polymorphism and runtime polymorphism. Compile time polymorphism, also known as static binding or early binding, is achieved by method overloading and operator overloading. Runtime polymorphism, also known as dynamic binding or late binding, is achieved by method overriding.

### Method overloading ###

It allows us to have the same method multiple times but with different parameters or return type.

    public class Program
    {  
        public int add(int a, int b)
        {  
            return a + b;  
        }  

        public int add(int a, int b, int c)  
        {  
            return a + b + c;  
        }  

        public float add(float a, float b)  
        {  
            return a + b;  
        }  
    } 

### Method overriding ###

It allows a class to override a method that is inherited from a base class. We need to use the keyword "virtual" in front of the method in the base class to allow a derived class to override it.

    public class Animal
    {  
        public virtual void play()
        {  
            Console.WriteLine("Playing");  
        }  
    }  

    public class Cat : Animal  
    {  
        public override void play()  
        {  
            Console.WriteLine("Playing like a cat");  
        }  
    }  

## Abstract classes ##

An abstract class is a class that cannot be instantiated. It can contain concrete or abstract methods. An abstract method is a method that has no body and it must be implemented by the derived class.

    public abstract class AbstractClass
    {  
        public abstract void AbstractMethod();

        public void Method()
        {
            Console.WriteLine("This is a method");  
        }  
    }  

    public class ConreteClass : AbstractClass 
    {  
        public override void AbstractMethod()  
        {  
            Console.WriteLine("Abstract method implementation");  
        }  
    }  

## Attributes ##

An attribute is a declarative tag that is used to send information to runtime about the behavior of various elements like classes, methods, structures, enumerators or assemblies. Attributes are used to add metadata, such as compiler instruction or other information to a program.

Attributes are generally applied physically in front of type and type member declarations. They're declared with square brackets, "[" and "]", surrounding the attribute. In the following example, we use the "Obsolete" attribute which is a pre-defined attribute in the _.NET Framework_.

    class Program
    {
        static void Main()
        {
            Test();
        }

        [Obsolete("Test method is obsolete.", true)]
        static void Test()
        {
        }
    }

When we try to compile the program, the compiler generates an error.

## Conclusion ##

Through this article we had an overview of the _C#_ programming language and how we can use it. It was more like a theoretical article more than a practical article. Nevertheless, we now have the tools to dig a little deeper and to use _C#_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!

