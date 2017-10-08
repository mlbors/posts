# The Composite Pattern #

Through this article we are going to see what the _Composite Pattern_ is. Let's get into it!

## Introduction ##

The _Composite Pattern_ is a _Structural Pattern_ that allows us to treat a group of objects the same way as a single instance of the object.

The formal definition of the _Composite Pattern_ says something like so (_Wikipedia_):

"_In software engineering, the composite pattern is a partitioning design pattern. The composite pattern describes that a group of objects is to be treated in the same way as a single instance of an object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly._"

This definition may seem to be a little hard to understand when we read it for the first time. So, another way to define this pattern using plain words can lead us to say that the _Composite Pattern_ lets clients treat the individual objects in a uniform manner.

This stay a little dark. A real-world example would be a company composed of employees from various types, and they all have a salary, some responsibilities and may or may not report to someone or have some subordinates. We can also see it as a program that can print some graphics and each of them can be made of just one line or be a composition of several graphics. We can also represent it as a program that manipulates a file system which is a tree structure that contains folders and files. Folders can contain one or more file or folder objects. Files and folders have many operations and attributes in common, that is why it is more convenient to treat them both uniformly.

## Definitions ##

_The Composite Pattern_ is made of the following objects:

### Component ###

_The Component_ is the abstraction for all _Components_, including _Composite_ ones. It declares the interface for objects in the composition. It can be an interface or an abstract class with some methods common to all the objects. It also declares an interface for accessing and managing its child _Components_.

### Leaf ###

A _Leaf_ has no children. So, it represents Leaf objects in the composition and implements all _Component_ methods. 

### Composite ###

It represents _Components_ that have children and it implements methods to manipulate children and all _Component_ methods, generally by delegating them to its children. It also provides additional methods for adding, removing and getting _Components_. 

### Client ###

The _Client_ manipulates objects in the hierarchy through the _Component_ interface.

## The Pattern ##

The aim of this pattern is to compose objects into tree structures to represent part-whole hierarchies. This pattern is also useful when we want the client to be able to ignore the difference between compositions of objects and individual objects.

The _Composite Pattern_ creates a class that contains a group of its own objects and this class provides ways to modify its group of same objects.

With this pattern, the _Client_ uses the _Component class interface_ to interact with objects in the composition structure. If the object is a _Leaf_, then the request is handled directly and if it is a _Composite_, then the request is forwarded to the child _Components_.

It is interesting to note that _Composite_ and _Decorator_ have similar structure diagrams. Nevertheless, _Decorator_ let us add responsibilities to objects without subclassing while _Composite_ focuses on representation. By the way, _Composite_ can use _Decorator_ to override the properties of the composition can be traversed with _Iterator_.

## Examples ##

We are going to make a couple of examples to illustrate this pattern.

### Form system ###

Let's say that we want to create a system that allows us to generate a web form, add many inputs as we want to it and then render it. We are going to use _PHP_ to make our example.

First, we create an interface for rendering, the _Component_:

	interface RenderableInterface
	{
		public function render(): string;
	}

Now, let's create some elements for our form, the _Leafs_:

	class InputElement implements RenderableInterface
	{

		public function render(): string
		{
			return '<input type="text" />';
		}
		
	}

	class TextElement implements RenderableInterface
	{

		private $text;
		
		public function __construct(string $text)
		{
			$this->text = $text;
		}
		
		public function render(): string
		{
			return $this->text;
		}
		
	}

We can now create the form itself, the _Composite_:

	class Form implements RenderableInterface
	{

		private $elements;

		public function render(): string
		{
			$form = '<form>';
			foreach ($this->elements as $element) {
				$form .= $element->render();
			}
			$form .= '</form>';
			return $form;
		}

		public function addElement(RenderableInterface $element)
		{
			$this->elements[] = $element;
		}
		
	}

We can use our system like so:

	$form = Form();
	$form->addElement(TextElement('First Name:'));
	$form->addElement(InputElement());
	$form->addElement(TextElement('Last Name:'));
	$form->addElement(InputElement());
	$form->render();

### Drawing program ###

Let's try to make a simple implementation of our drawing program. Here, we are going to use _Java_.

First, we create an interface for the shapes, the _Component_:

	public interface Shape {
		public void draw();
	}

Now, let's create a _Leaf_ element:

	public class Line implements Shape {

		public Line(int x1, int y1, int x2, int y2) {
			// Do something or not
		}

		public void draw() {
			System.out.println("Line at " + x1 + " " + y1 + " " + x2 + " " + y2);
		}

	}

And now we create a complex shape, a _Composite_:

	public class Rectangle implements Shape {

		Shape[] edges = {new Line(-1,-1,1,-1), new Line(-1,1,1,1), new Line(-1,-1,-1,1), new Line(1,-1,1,1)};
		
		public void draw() {

			for (Shape s : edges) {
				s.draw();
				
			}
			
		}

	}

The _Client_ could be something like so:

	public class editor {

		public static void main(String[] args) {
		
			List shapes = new ArrayList();
		
			Shape line = new Line(0,0,1,1);
			Shape rectangle = new Rectangle();
			
			shapes.add(line);
			shapes.add(rectangle);
			
			render(shapes);
		
		}
		
		private static void render(List shapes) {
		
			for (Shape s : shapes){
				s.draw();
			}
		
		}

	}

## Conclusion ##

In this post, we saw what the _Composite Pattern_ is and how it works. We saw that it relies on four elements: _Component_, _Leaf_, _Composite_ and _Client_. This pattern lets clients treat individual objects in a uniform manner.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!