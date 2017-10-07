# A small introduction to Linked Lists #

A _linked list_ is a linear collection of data elements, which are connected together via _links_. Let’s have a quick look at it!

A _linked list_ is a sequence of data items, just like an array. The main difference between an array and a _linked list_ is while an array needs a big block of memory to store the objects, the elements in a _linked list_ are totally separate objects in memory and are connected through _links_.

The elements of a _linked list_ are referred to as _nodes_ and each _node_ has a _reference_ (or “pointer”) to the next _node_. We need to keep track of where the list begins and it is usually done with a _pointer_ called the _head_. It also can be useful to have a _reference_ to the end of the list called the _tail_. _The next pointer of the last node is nil_, just like _the previous pointer of the first node_.
    
    
    | node 0 |--->| node 1 |--->| node 2 |--->| node 3 |

We can also have something called _doubly linked list_, where _each node has a pointer to the previous node_.

A _singly linked list_ uses less memory than a _doubly linked list_ because it does not need to store all _previous pointers_. However, with a _singly linked list_, if we need to find the _previous pointer of any node_, we have to start at the _head_ of the list and iterate through the entire list until we get to the right _node_.

Most operations on a _linked list_ have _O(n)_ time, and so _linked lists_ are generally slower than arrays. The main reason for that is we can’t access directly to a specific _node_. If we don’t have a _reference_ to that specific _node_ already, we have to start at the _head_ (or at the tail) of the list and work our way down (or up) to the desired _node_ by following the _next pointers_ (our previous).

Nevertheless, once we have a reference to a _node_, operations like _insertion_ and _deletion_ are really fast.

Let’s have a really simple example written in _Java _of a _singly list_ implementation. For the sake of our example, we will work with books and we will define everything as _public_ so _getters_ and _setters_ aren’t needed.

First of all, we define a _LinkList class_ like so:
    
    
    public class LinkList {
    	
    	public Link firstLink;
    	
    	LinkList() {
    		firstLink = null;
    	}
    	
    	public boolean isEmpty() {
    		return(firstLink == null);
    	}
    	
    	public void insertFirstLink(String bookName) {
    		
    		Link newLink = new Link(bookName);
    		newLink.next = firstLink;
    		firstLink = newLink;
    		
    	}
    	
    	public Link removeFirst() {
    		
    		Link linkReference = firstLink;
    		
    		if (!isEmpty()) {
    			firstLink = firstLink.next;
    		} else {
    			System.out.println("Empty LinkedList");
    		}
    		
    		return linkReference;
    		
    	}
    	
    	public void display() {
    		
    		Link theLink = firstLink;
    		
    		while(theLink != null) {
    			
    			theLink.display();
    			System.out.println("Next link: " + theLink.next);
    			theLink = theLink.next;
    			System.out.println();
    			
    		}
    		
    	}
    	
    	public Link find(String bookName) {
    		
    		Link theLink = firstLink;
    		
    		if (!isEmpty()) {
    			
    			while(theLink.bookName != bookName ) {
    				
    				if (theLink.next == null) {
    					return null;
    				} else {
    					theLink = theLink.next;
    				}
    				
    			}
    			
    		} else {
    			System.out.println("Empty LinkedList");
    		}
    		
    		return theLink;
    		
    	}
    	
    	public Link removeLink(String bookName) {
    		
    		Link currentLink = firstLink;
    		Link previousLink = firstLink;
    		
    		while(currentLink.bookName != bookName ) {
    			
    			if (currentLink.next == null) {
    				return null;
    			} else {
    				previousLink = currentLink;
    				currentLink = currentLink.next;
    			}
    			
    		}
    		
    		if (currentLink == firstLink) {
    			firstLink = firstLink.next;
    		} else {
    			previousLink.next = currentLink.next;
    		}
    		
    		return currentLink;
    		
    	}
    	
    }

Then we define a _Link class_ and do some basic operations in the _main_ just to illustrate our example:
    
    
    public class Link {
    	
    	public String bookName;
    	
    	public Link next;
    	
    	public Link(String bookName) {
    		this.bookName = bookName;
    	}
    	
    	public void display() {
    		System.out.println(bookName);
    	}
    	
    	public String toString() {
    		return bookName;
    	}
    	
    	public static void main(String[] args ) {
    		
    		LinkList theLinkedList = new LinkList();
    		
    		theLinkedList.insertFirstLink("2001 A Space Odyssey");
    		theLinkedList.insertFirstLink("Twenty Thousand Leagues Under the Sea");
    		theLinkedList.insertFirstLink("The Lord of the Rings");
    		theLinkedList.insertFirstLink("1984";
    		
    		theLinkedList.display();
    		theLinkedList.removeFirst();
    		theLinkedList.display();
    		
    		System.out.println(theLinkedList.find("The Lord of the Rings").bookName + " was found");
    		theLinkedList.removeLink("The Lord of the Rings");
    		theLinkedList.display();
    		
    	}
    	
    }
