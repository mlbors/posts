# Some thoughts about SharePoint and Unit Testing #

Through this post, we are going to try to have a reflection about _SharePoint_ and _Unit Testing_.

## Introduction ##

Let's be honest: here, we are not going to picture the perfect solution to write good unit tests easily when we use _SharePoint_ as a development platform. Writing unit tests when we develop something for _SharePoint_ can be really hard and discouraging. However, we are going to overview a few options to achieve this and to have a better conscience (or not).

Whatever development model we choose, we will face some problems and scratch our head to the bone. Some even say that _SharePoint_ was not designed with testability in mind. However, let's see various ways we can explore.

## Option 1: avoid Unit Testing ##

This option is pretty radical and simple. It depends on whether we can live with it our not.

## Option 2: use third-party tools dedicated to SharePoint ##

There are several tools dedicated to _SharePoint_ that can help us to achieve _Unit Testing_ against _SharePoint_. However, good ones required to use our credit card and don't assure that everything will go smoothly.

## Option 3: wrap SharePoint objects ##

When we develop using the _.NET Framework_ and want to write our various tests, it is really common to use tools such as Moq to create fake objects to easily isolate what we want to test. Now, with _SharePoint_, our main problem is the code that depends on _SharePoint_. Using Moq to mock _SharePoint_ will most of the time lead us to a dead end. _SharePoint_ classes are often sealed and some objects cannot be instantiated without an _HTTP Context_. Maybe we will succeed to mock a few things, but the result won't be satisfying and will probably be messy.

One workaround to that problem is to wrap the values of the various _SharePoint_ objects we need in classes or structs that we control and can easily mock. This will require to create extra classes, but, if we use these various wrappers, it can ease _Unit Testing_.

## Option 4: create another layer ##

This option makes use of "_Option 3_" and leads us to create another layer between our code and _SharePoint_. It means that instead of using the various _SharePoint_ APIs directly, we create one or more objects (_Services_, _Proxies_ or whatever we want to call it) that we will work with. These objects will then work with the various _SharePoint_ APIs, directly or through Repositories, it depends on how we want to implement this concept, and return wrapped objects.

With this solution, it means that we can concentrate usage of _SharePoint_ objects in a restricted area and decouple our code from _SharePoint_ APIs. So, it means in things like _Event Receivers_ or code behind _Control Templates_, instead of using _SharePoint_ classes we use our different _Services_. It makes our code more testable and avoid code duplication.

However, most of the methods exposed by _SharePoint_ objects don't have a return value. So, if we have a _Service_ that communicates with a _Repository_, how can we know that _SharePoint_ failed or succeeded in achieving the requested operation? Well, sadly, we have to find workarounds. For example, when we add an _SPItem_ to an _SPList_, we can count the number of items before and after the operation and check if the item exists in the updated collection and return a boolean value depending on the scenario. This leads to extra code and to extend the time of the operation, but we will have an answer.

Of course, this option could lead to over-engineering problems and there will always be a point where we will face _SharePoint_ objects. We also have to take extra care of the _SPContext_, _SPSite_ and _SPWeb_ objects handling because it could raise sever exceptions if we do it without caution.

## Option 5: create a console application ##

This is not really _Unit Testing_. However, we can imagine creating a small console application using _C#_ or _PowerShell_ that will check, after we deployed our package, if our various _Features_ were installed and activated or if our _Lists_ are in the right place. It involves the whole _SharePoint_ installation and "real" data.

## Conclusion ##

Through this article, we explored some options we have when we want to _Unit Testing_ against _SharePoint_. We can see that _Unit Testing_, in such a case, is not really easy and can bring us pain and suffering, and maybe more than if we would do it with another platform or framework. However, we have a few possibilities than can make us more secured with our development. It is up to us to decide which solution is the best depending on what we want to achieve and to accept that some things can only best test by hand and to remember this good old "_try...catch_" thing is here for us.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!