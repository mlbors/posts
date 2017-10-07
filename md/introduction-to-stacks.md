Through this small post, we are going to see what _Stacks_ are.

A _Stack_ is a _data structure_ that behaves like a real-world stack, for example a deck of cards. And like in real-world, we can only add a new element to the _top_ of the _Stack_ or remove the element from the _top_ of the _Stack_.

A _Stack_ keeps elements in a _last-in first-out order_ (LIFO). A _Stack_ can be implemented using _Array, Structure, Pointer_ or _Linked List_.

When we add an element to the _Stack_, we _push_ and when we remove the element from the _top_, we _pop_. We can also _peek_ to get the element from the _top_ without removing it.

The _push_ and _pop_ operations occur only at one end of the structure: the _top_ of the _Stack_. If we want, we can constraint the _Stack_ to have a fixed size and when it is full, so no entity can be pushed to the _top_ anymore, it is considered to be in an _overflow_ state.

Let's make a quick and small implementation using _Java_ where we will use _Array_ and a fixed size.

	public class TheStack {
		
		private String[] stackArray;
		private int stackSize;
		private int topOfStack = -1;
		
		TheStack(int size) {
			
			stackSize = size;
			stackArray = new String[size];
			Arrays.fill(stackArray, "-1");
			
		}
		
		// Push operation
		public void push(String input) {
			
			if(topOfStack + 1 < stackSize) {
				
				topOfStack++;
				stackArray[topOfStack] = input;
				System.out.println("PUSH: " + input + "was added to the Stack.");
				
			} else {
				System.out.println("The Stack is full!");
			}
		
		}
		
		// Pop operation
		public String pop() {
			
			if(topOfStack >= 0){
				
				System.out.println("POP: " + stackArray[topOfStack] + " was removed from the Stack.");
				stackArray[topOfStack] = "-1";
				return stackArray[topOfStack--];
		
			} else {
				
				displayTheStack();
				System.out.println("Sorry but the stack is empty");
				return "-1";
			}
			
		}
		
		// Peek operation
		public String peek() {
			
			System.out.println("PEEK: " + stackArray[topOfStack] + " is at the top of the Stack.");
			return stackArray[topOfStack];
			
		}
		
		// Using our Stack
		public static void main(String[] args ) {
			
			TheStack theStack = new TheStack(10);
			theStack.push("10");
			theStack.push("15");
			theStack.peek();
			theStack.pop();
			
		}
		
	}

We can also do it with _Python_ using _Linked List_ just for pleasure:

	# The Node class for our Linked List
	class Node(object):

		def __init__(self, data, next=None):
			self.data = data
			self.next = next


	# Our Stack class
	class Stack(object):

		def __init__(self, top = None):
			self.top = top

		# Push operation
		def push(self, data):
			self.top = Node(data, self.top)

		# Pop operation
		def pop(self):
			if self.top is None:
				return None
			data = self.top.data
			self.top = self.top.next
			return data

		# Peek operation
		def peek(self):
			return self.top.data if self.top is not None else None

		def is_empty(self):
			return self.peek() is None


	# Using our Stack
	if __name__ == '__main__':
		stack = Stack()
		stack.push(10)
		stack.push(15)
		print(stack.pop())
		print(stack.peek())