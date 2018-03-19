# Async and Await in C\# #
 
In this article, we are going to look at the keywords "_async_" and "_await_" in _C#_. Let's get into it!

## Introduction ##

Using asynchronous programming allows us to enhance the responsiveness of our application. It helps us in case we have a potentially blocking activity where the entire application has to wait before continuing. Using an asynchronous process lets the application continue its work that doesn't depend on the blocking activity until this task is finished. Let's see how we can put that in place.

## Tasks ##

To achieve asynchronous programming with _C#_, we need to be aware of _Tasks_.

A _Task_ represents an asynchronous operation. It is an operation we want to perform and that is executed in the background. A _Task_ is something like a _promise_, or a _future_, that says something like "_I will return this a little bit later_". _Tasks_ can be chained to be executed one after the other.

A _Task_ mustn't be confused with a _Thread_. _Thread_ is a lower-level concept. A _Thread_ is a way of fulfilling a promise, but not every _Task_ needs a brand-new _Thread_. A _Task_ can return a result, while there is no direct mechanism to return a result from a _Thread_.

If we are familiar with _JavaScript Promises_, _Tasks_ are quite the same.

## Example and explanation ##

Let's imagine the following code:

    private async void Button_Click(object sender, RoutedEventArgs e)
    {
        textBox.Text = "Wait...";
        // ... DOING SOME OPERATIONS
        string result = await SimpleMethodAsync();
        textBox.Text = result;
    }

    public async Task<string> SimpleMethodAsync()
    {
        // ... DOING SOMETHING
        await Task.Delay(2000);
        return "Finished";
    }

As we can see, here we use three words that we had already mentioned: "_async_", "_await_", "_Task_". So, what does happen here? We can see that we use the keyword "_async_" in front of the "_Button\_Click()_" method. It says that we define an asynchronous method. This also means that we are able to wait for something. So, when the method "_Button\_Click()_" is called, responding to a click event, it is executed synchronously until it reaches the keyword "_await_". By that time, the "_SimpleMethodAsync()_" will be called and if other independent work can be done, it is operated. The "_await_" keyword tells the compiler that we ultimately need the result of the "_SimpleMethodAsync()_" method, but we don't need to block on that call.

The "_SimpleMethodAsync()_" also has the "_async_" keyword in front of it. This method returns a _string_ and we specify it with the return type "_Task\<string\>_". After our "return" statement, the "_Button\_Click()_" method can get the result.

An important note is that even though returning "_void_" in an "_async_" method is allowed, it should not be used in most cases. The other two return types, "_Task_" and "Task\<T\>", represent "_void_" and "_T_", after the _awaitable_ method completes and returns result. The use of void as return type should be only limited for _event handlers_.

## In other words ##

The "_async_" keyword enables the "_await_" keyword in a method and changes how the method result is handled. An "_async_" method is executed like any other method, synchronously, until it reaches the "_await_" keyword. At this point, things get asynchronous. "_await_" takes a single argument, an "_awaitable_", something that is an asynchronous operation. "_await_" check if the "_awaitable_" has already completed and if it is the case, the method just continues synchronously. Otherwise, it tells the "_awaitable_" to run the remainder of the method when it completes, and then returns from the "_async_" method. When the "_awaitable_" completes, the remainder of the "_async_" method is executed.

## Conclusion ##

Through this article, we saw the meaning of the "_async_" and "_await_" keywords in _C#_. We had a basic overview of how asynchronous operations work and how we can use them.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!