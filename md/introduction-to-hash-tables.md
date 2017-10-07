# Introduction to Hash Tables #

Through this article, we are going to take a look at the data structure called _Hash Table_.

## Hash Table

A _Hash Table_ is a data structure that stores data in an associative manner. It is made up of two parts: an array, where the data is stored, and a _Hash Function_ which is a mapping function. Basically, a _Hash Function_ is a function that takes things from one space and maps them to a space for indexing.

A _Hash Table_ is used to implement structures such as _dictionary_, _map_, or _associative array_ and it is a data structure in which insertion and search operations are very fast.

The idea behind a _Hash Table_ is that for each element we want to store, we calculate a unique address and we put the value at this index in the array. When we need to find a value, we once again calculate its index and then return the value. In other words, a _Hash Table_ allows us to store and retrieve objects by key.

A _Hash Table_ is an array and initially, this array is empty. When we want to put a value into the _Hash Table_ under a certain key, that key will be used to compute an index in the array. We can see it like so:

    hashTable["firstName"] = "John"

        hashTable array:
        +--------------+
        | 0:           |
        +--------------+
        | 1:           |
        +--------------+
        | 2: firstName | ---> John
        +--------------+
        | 3:           |
        +--------------+
        | 4:           |
        +--------------+

Here, the key is "_firstName_" and it maps to index _2_. It means that our _Hash Table_ asked for the index of the key "_firstName_" and the _Hash Function_ returned _2_. We can see it like so:

    key ---> HASH FUNCTION --> array_index

## Hash Function

_Hashing_ is a technique that lets us convert a range of key values into a range of indexes of an array. The values returned by a _Hash Function_ are called _Hash Values_. There are multiple ways for constructing a _Hash Function_ but it is important that the function has some basic requirements:

* The _Hash Value_ is fully determined by the data being hashed and it should be easy to compute
* The _Hash Function_ uses all the input data
* The _Hash Function_ should provide a uniform distribution across the _Hash Table_
* The _Hash Function_ generates very different _Hash Values_ for similar strings

## Solving collisions

Let's say that our _Hash Function_ will compute the index of a specific string by summing the _ASCII_ values of the characters modulo _373_ (that is a prime number). Our set of string will be _{"abcdef", "bcdefa", "cdefab" , "defabc"}_. Knowing that the _ASCII_ values of _a_, _b_, _c_, _d_, _e_, and _f_ are _97_, _98_, _99_, _100_, _101_, and _102_, our addition will always give us the same result (_597_) just like the operation _597%373_.

When different elements get the same _Hash Value_, it is a called a _collision_. So, how can we handle such a situation? The two most popular ways to solve this are called _Chaining_ and _Open Addressing_.

### Chaining

When we use this method, we just use a _Linked List_ and put all the values with the same _Hash Value_ into it and so the _Hash Table_ becomes an array of _Linked Lists_.

### Open Addressing

Also called _Linear Probing_, when we use this method and we want to add a new entry, the _Hash Index_ of the _Hashed Value_ is computed and then the array is examined. If the slot is empty, then the value is inserted. If not, we calculate another _Hash Value_ and check that slot and we continue until a place for the value is found. The probe sequence will be like so:

    index = index % hashTableSize
    index = (index + 1) % hashTableSize
    index = (index + 2) % hashTableSize
    index = (index + 3) % hashTableSize
    ...

### Quadratic Probing

_Quadratic Probing_ is similar to _Linear Probing_ but the difference is the interval between successive probes. With _Quadratic Probing_, the sequence would be something like this:

    index = index % hashTableSize
    index = (index + 1^2) % hashTableSize
    index = (index + 2^2) % hashTableSize
    index = (index + 3^2) % hashTableSize
    ...

### Double Hashing

_Double Hashing_ works on a similar idea to _Linear_ and _Quadratic Probing_. The difference is that a second _Hash Function_ is used to determine the location of the next empty slot. The sequence would look like this:

    index = (h1(key) + 1 * h2(key)) % hashTableSize;
    index = (h1(key) + 2 * h2(key)) % hashTableSize;
    index = (h1(key) + 3 * h2(key)) % hashTableSize;
    ...

## Basic Examples

Knowing what we know, it is time to make really simple examples using _Java_.

### Linear Probing

First, let's define a class for the _Entry_:

    public class HashEntry {

        private int key;
        private int value;

        HashEntry(int key, int value) {
                this.key = key;
                this.value = value;
        }

        public int getKey() {
                return key;
        }

        public int getValue() {
                return value;
        }
    }

And now, let's create our _Hash Table_:

    public class HashTable {

        private final static int TABLE_SIZE = 64;

        HashEntry[] table;

        HashTable() {
            table = new HashEntry[TABLE_SIZE];
            for (int i = 0; i < TABLE_SIZE; i++) {
                table[i] = null;
            }
        }

        public int get(int key) {
            int hash = (key % TABLE_SIZE);

            while (table[hash] != null && table[hash].getKey() != key) {
                hash = (hash + 1) % TABLE_SIZE;
            }

            if (table[hash] == null) {
                return -1;
            } else {
                return table[hash].getValue();
            }
        }

        public void put(int key, int value) {
            int hash = (key % TABLE_SIZE);

            while (table[hash] != null && table[hash].getKey() != key) {
                hash = (hash + 1) % TABLE_SIZE;
            }
            table[hash] = new HashEntry(key, value);
        }
    }

### Chaining

First, we define the _Nodes_ of our _Linked List_.

    public class HashNode {

        private int key;
        private int value;
        private HashNode next;

        HashNode(int key, int value) {
            this.key = key;
            this.value = value;
            this.next = null;
        }

        public int getValue() {
            return value;
        }

        public void setValue(int value) {
            this.value = value;
        }

        public int getKey() {
            return key;
        }

        public HashNode getNext() {
            return next;
        }

        public void setNext(HashNode next) {
            this.next = next;
        }
    }

Now, let's create our _Hash Table_:

    public class HashTable {

        private final static int TABLE_SIZE = 64;

        HashNode[] table;

        HashTable() {
            table = new HashNode[TABLE_SIZE];
            for (int i = 0; i < TABLE_SIZE; i++) {
                table[i] = null;
            }
        }

        public int get(int key) {
            int hash = (key % TABLE_SIZE);

            if (table[hash] == null) {
                return -1;
            } else {
                HashNode entry = table[hash];

                while (entry != null && entry.getKey() != key) {
                    entry = entry.getNext();
                }

                if (entry == null) {
                    return -1;
                } else {
                    return entry.getValue();
                }
            }
        }

        public void put(int key, int value) {
            int hash = (key % TABLE_SIZE);

            if (table[hash] == null) {
                table[hash] = new HashNode(key, value);
            } else {

                HashNode entry = table[hash];

                while (entry.getNext() != null && entry.getKey() != key) {
                    entry = entry.getNext();
                }

                if (entry.getKey() == key) {
                    entry.setValue(value);
                } else {
                    entry.setNext(new HashNode(key, value));
                }
            }

        }
    }

## Conclusion

Through this article, we saw what are _Hash Tables_, how they work and what problem we could have when we use them. It is also good to know that there are many existing well-tested _Hash Functions_ that we can rely on.