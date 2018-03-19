# Pointers & References #

In this article, we are going to talk about _pointers_ and _references_.

## Introduction ##

_Pointers_ and _references_ can be something that confuses us quickly, mostly if we are not really familiar with _C++_. Let's take a little look at these different notions. Here, we are going to use _C++_ through our various examples.

## Memory ##

We know that a _variable_ is some kind of box where we put a value. A _variable_ is a location in the computer's memory and it can be accessed by its name, or identifier. For a program, the computer's memory is a set of cells and each of these cells has a size and a unique address. Cells are ordered in a specific order that makes, for example, the cell with the address _1234_ always follows immediately after the cell with the address _1233_ and precedes the cell with the address _1235_. We can think of some kind of very long street where all the houses are on the same side and are sorted in an ascending way.

When we declare a _variable_, the required memory to store the value is assigned to a specific location in the memory. So, our value has now a memory address. This task of assignment is left to the execution environment.

Now, it would be useful if we could get the address of a _variable_ during runtime to achieve some operations. But how can we achieve such a thing? Let's go a little further.

## Pointers: \* ##

A _pointer_ _variable_, or more simply, a _pointer_, is a _variable_ that stores a memory address. We can declare a _pointer_ by using the "_\*_" symbol.

    // Declaring an integer
    int a = 5;
    // Declaring a pointer
    int * b = &a;
    // Printing the result: memory address
    cout << b;

Here, we get a result like "_0x7ffd9931be2c_". We can also see that our _pointer_ is associated with a type, here, an _integer_.

## Address-of operator: & ##

In our previous example, we used another symbol: "_&_". This is the "_address-of_" operator. This operator returns the memory address of a _variable_.

In our previous example, we could have just done something like this:

    int * b;

This possible, but the content of our _pointer_ is not initialized. Here, the value of our _pointer_ would point to "_somewhere_", which is not a valid location.

## Dereferencing operator ##

The "_dereferencing_" operator, or "_indirection_" operator, "_\*_", operates on a _pointer_. It returns the value stored in the address placed into a _pointer_ _variable_.

    // Declaring an integer
    int a = 5;
    // Declaring a pointer
    int * b = &a;
    // Getting the value stored in the address kept by b
    int c = *b;
    // Printing the result: 5
    cout << c;

## References ##

A _reference_ _variable_, or just _reference_, is an alias of an existing _variable_. It means if we make the _variable_ "_b_" a _reference_ to "_a_", we can refer to this _variable_ by using both names.

A _reference_ allows us to achieve something called "pass-by-_reference_". It means that when a _reference_ is passed to a function, the function will work with the original _variable_ that was passed, not a copy. So, if a change is made inside the function, it is also reflected outside the function.

To declare a _reference_, we use the same symbol we use for the "_address-of_" operator, "_&_".

    // Declaring an integer
    int a = 5;
    // Declaring a reference
    int & b = a;
    // Modifying the value
    b = 6;
    // Printing a: 6
    cout << a;

But, how does a _reference_ work? It is like a _pointer_ and it stores a memory address.

## Pointers VS References ##

So, what are the differences between _pointers_ and _references_?

In case of a _pointer_, we store the address of a _variable_ while in case of a _reference_, we create an alias for the original _variable_, so the same address is used by two variables with different names. In other words, a _pointer_ has its own memory address and size, while a _reference_ has the same address as the original _variable_ itself.

To get the value pointed by a _pointer_, we need to use the dereferencing operator "_\*_", and to assign an address of a _variable_ to a _pointer_, we need to use the "_address-of_" operator "_&_".

For _references_, referencing and dereferencing operations are done implicitly.

A _pointer_ can be re-assigned while a _reference_ can't and must be assigned at initialization:

    // Pointers
    int a = 5;
    int b = 6;
    int * p;
    p = &a;
    cout << p << endl;
    p = &b;
    cout << p << endl;

    // References
    int c = 7;
    int d = 8;
    int & r = c;
    cout << r << endl;

    int & r2; // ERROR !!!
    r2 = d;
    cout << r2 << endl;   

A _pointer_ can be "_NULL_" while a _reference_ can't:

    // Pointers
    int *p = NULL;
    cout << p << endl;

    // References
    int & r = NULL; // ERROR !!!
    cout << r << endl;

We can have something we call "_pointer to pointer_" that allows us to preserve the Memory-Allocation or Assignment even outside of a function call: 

    int a = 4;
    int b = 5;
    int *p1 = &a;
    int *p2 = &b;

    cout << p1 << " - " << *p1 << endl; // Print: 0x7ffe3b49bfe8 - 4 
    cout << p2 << " - " << *p2 << endl; // Print: 0x7ffe3b49bfec - 5  

    int **pp = &p1;

    cout << pp << " - " << *pp << " - " << **pp << endl; // Print: 0x7ffe3b49bff0 - 0x7ffe3b49bfe8 - 4     

    pp = &p2;

    cout << pp << " - " << *pp << " - " << **pp << endl; // Print: 0x7ffe3b49bff8 - 0x7ffe3b49bfec - 5  

    **pp = 6;

    cout << pp << " - " << *pp << " - " << **pp << endl; // Print: 0x7ffe3b49bff8 - 0x7ffe3b49bfec - 6           
    cout << p1 << " - " << *p1 << endl; // Print: 0x7ffe3b49bfe8 - 4 
    cout << p2 << " - " << *p2 << endl; // Print: 0x7ffe3b49bfec - 6    

## Conclusion ##

Through this article, we saw what _pointers_ and _references_ are. We saw that a _pointer_ is a _variable_ that stores a memory address while a _reference_ is an alias of an existing _variable_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!