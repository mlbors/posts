Through this small article, we are going to see what _Queues_ are. Let's get into it!

A _Queue_ is a _data structure_ that is really similar to a real-world queue. A _Queue_ is open at both ends and one end is used to insert data while the other is used to remove data.

A _Queue_ keeps elements in _first-in first-out_ order (FIFO), so the first element we inserted is the first one to come out.

In a way, _Queues_ are similar to _Stacks_ and can be implemented using _Array_, _Structure_, _Pointer_ or _Linked List_.

When we add an item to the _Queue_, we _enqueue_ and when we remove an item from the _Queue_, we dequeue. We can also peek to get the element at the front of the _Queue_ without removing it.

Let’s make a quick and small implementation using _Java_ where we will use _Array_.

	public class TheQueue {
		
		private String[] queueArray;
		private int queueSize;
		
		private int front, rear, numberOfItems = 0;
		
		TheQueue(int size) {
			
			queueSize = size;
			queueArray = new String[size];
			front = 0;
			rear = -1;
			numberOfItems = 0;
			
		}
		
		// Enqueue operation
		public void enqueue(String input) {
			
			if(numberOfItems + 1 <= queueSize) {
				
				rear++;
			
				queueArray[rear] = input;
				
				numberOfItems++;
				
				System.out.println("ENQUEUE: " + input + " was added to the Queue.");
			
			} else {
				
				System.out.println("The Queue is full.");
				
			}
			
		}
		
		// Dequeue operation
		public void dequeue() {
			
			if(numberOfItems > 0) {
				
				System.out.println("DEQUEUE: " + queueArray[front] + " was removed from the Queue.");
				
				queueArray[front] = "-1";
				
				front++;
			
				numberOfItems--;
			
			} else {
				
				System.out.println("The Queue is empty");
				
			}
			
		}
		
		// Peek front operation
		public void peekFront() {
			
			System.out.println("PEEK FRONT: " + queueArray[front]);
			
		}
		
		// Peek rear operation
		public void peekRear() {
			
			System.out.println("PEEK REAR: " + queueArray[rear]);
		}
		
		
		// Using our Queue
		public static void main(String[] args){
			
			TheQueue theQueue = new TheQueue(10);
			
			theQueue.enqueue("45");
			theQueue.enqueue("100");
			theQueue.enqueue("98");
			theQueue.enqueue("25");
			theQueue.enqueue("11");
			theQueue.peekFront();
			theQueue.peekRear();
			theQueue.dequeue();
			theQueue.dequeue();
			theQueue.peekFront();
			theQueue.peekRear();
			
		}

	}

We can also do something similar using _Python_:

	class Queue:
		def __init__(self, size):
			self.queue = []
			self.size = size

		# Enqueue operation
		def enqueue(self, item):
			if len(self.queue) < self.size:
				self.queue.append(item)
				return self.queue
			else:
				return "The Queue is full."

		# Deqeueue operation
		def dequeue(self):
			if len(self.queue) > 0:
				return self.queue.pop(0)
			else:
				return "The Queue is empty."


	# Using our Queue
	if __name__ == '__main__':
		queue = Queue(10)
		queue.enqueue("45")
		queue.enqueue("100")
		queue.enqueue("98")
		queue.enqueue("25")
		queue.enqueue("11")
		print(queue.queue)
		queue.dequeue()
		queue.dequeue()
		print(queue.queue)