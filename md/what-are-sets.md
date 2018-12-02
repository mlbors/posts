# What are Sets? #

In this small article, we are going to take a look at _Sets_ to understand what they are and how they work.

## Introduction ##

In computer science, a _Set_ is a data structure. It can store unique values of the same type without any particular order. However, the stored objects have to be comparable.

A _Set_ is an implementation of the mathematical concept of a finite set.

Here, we are going to build our own _Set_ class using _C#_ and _.NET Framework_ to understand how it works. First, we are going to see the basic set up for our class. Then, we are going to examine the specific operations _Union_, _Intersection_, _Difference_ and _Symmetric Difference_. Nevertheless, this _Set_ class is not going to be a production quality data structure.

## Set Class ##

Let's imagine the following code:

    public class Set<T> : IEnumerable<T> where T: IComparable<T>
    {
        private readonly List<T> _items = new List<T>();

        public Set()
        {
        }

        public Set(IEnumerable<T> items)
        {
            AddRange(items);
        }

        public void Add(T item)
        {
            if (Contains(item))
            {
                return;
            }

            _items.Add(item);
        }

        public void AddRange(IEnumerable<T> items)
        {
            foreach (T item in items)
            {
                Add(item);
            }
        }

        public bool Remove(T item)
        {
            return _items.Remove(item);
        }

        public bool Contains(T item)
        {
            return _items.Contains(item);
        }

        public int Count
        {
            get
            {
                return _items.Count;
            }
        }

        public Set<T> Union(Set<T> otherSet)
        {
            Set<T> result = new Set<T>(_items);
            result.AddRange(otherSet._items);

            return result;
        }

        public Set<T> Intersection(Set<T> otherSet)
        {
            Set<T> result = new Set<T>();

            foreach (T item in _items)
            {
                if (otherSet._items.Contains(item))
                {
                    result.Add(item);
                }
            }

            return result;
        }

        public Set<T> Difference(Set<T> otherSet)
        {
            Set<T> result = new Set<T>(_items);

            foreach (T item in otherSet._items)
            {
                result.Remove(item);
            }

            return result;
        }

        public Set<T> SymmetricDifference(Set<T> otherSet)
        {
            Set<T> intersection = Intersection(otherSet);
            Set<T> union = Union(otherSet);
            return union.Difference(intersection);
        }

        public IEnumerator<T> GetEnumerator()
        {
            return _items.GetEnumerator();
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _items.GetEnumerator();
        }
    }

As we can see, we implemented our own _Set_ class. Let's examine it in detail.

## Constraints, members and Construct ##

As we can see, our class is a generic class. We also decided that our class has to implement the _IEnumerable_ interface. Finally, we added a constraint to tell that "_T_" has to be a comparable object.

We chose to use the "_List_" data structure to store our values, but if we wanted, we could have done it with an array.

We declared two constructs, one without any arguments, and another one that lets us initialize our _Set_ with a collection of values.

## Basic operations ##

Let's see the basic operations we need to use our Set.

First, the _Add_ method simply checks if the value we want to add to our _Set_ is already in our list of items. If it is not, it adds this value to the list.

_Remove_ and _Contains_ do what they are supposed to do. Here, we just use the methods provided by the "_List_" class. We also do the same for the _Count_ method.

## Union ##

_Union_ compares two _Sets_ and returns a third _Set_ that contains all of the unique elements in both _Sets_. So, for example the _Union_ of {1,3,4,9} and {3,6,7} is {1,3,4,6,7,9}.

## Intersection ##

_Intersection_ compares two _Sets_ and returns a third _Set_ that contains all the values shared by both _Sets_. The _Intersection_ of {1,2,7} and {2,4,7,9} is {2,7}.

## Difference ##

_Difference_ compares two _Sets_ and returns a third _Set_ that contains the values that are only in the first Set. So, the _Difference_ of {1,3,5,6} and {2,3,6,7} is {1,5}.

## Symmetric Difference ##

_Symmetric Difference_ is the _Difference_ of the _Union_ and the _Intersection_. In other words, it compares two _Sets_ and returns a third _Set_ that contains values that are only in one _Set_. So the _Symmetric Difference_ of {1,2,4,6,7} and {1,3,4,5,9} is {2,3,5,6,7}.

## Conclusion ##

Through this small post, we saw what _Sets_ are and how they work. We saw how we can implement our own _Set_ class and which concepts lie behind this data structure.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!
