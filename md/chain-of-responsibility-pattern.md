# Chain of Responsibility Pattern #

Through this article we are going to take a look at the _Chain of Responsibility Pattern_. Let's get into!

## Introduction ##

The _Chain of Responsibility Pattern_ is _Behavioural Pattern_ that is defined like so:

"_Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it._"

A real world example of this pattern would be a king that gives orders to his army. The closest element to react would be the commander, then the officer and then the soldier and those three elements would form a _Chain of Responsibility_.

## The Pattern ##

In plain words, this pattern helps to build a chain of objects and allows an object to send a request without knowing what object will receive and handle it. The request enters from one end and keeps going from object to object until it finds the suitable handler.

In the _Chain of Responsibility Pattern_, each receiver contains a reference to another receiver. If one object cannot handle the request, then it passes the same request to the next receiver and so on.

_Chain of Responsibility_ can use _Command_ to represent requests as objects and it is also often used with _Composite_.

## Structure ##

This pattern needs the following things:

### Handler ###

It defines an interface for handling requests. It can also implement the successor link.

### Request Handler ###

It handles the requests it is responsible for. If it can handle the request it does so, otherwise it forwards the request to its successor.

### Client ###

It sends commands to the first object in the chain that may handle the command.

## Example ##

Let's make a simple example to illustrate how we can use this pattern. We are going to us _PHP_ for this and we are going to say we are building some kind of service.

First, we define an interface (_Handler_):

	interface Handler
	{
		public function handleRequest($request); 
		public function setSuccessor($nextService);		
	}

Then we can have two concrete implementations (_Request Handlers_):

	class JsonService implements Handler
	{

		private $successor;
	 
		public function setSuccessor($nextService)
		{
			$this->successor = $nextService;
		}
	 
		public function handleRequest($request)
		{
			if ($request->getService() == "JSON")
			{
				echo ("I am parsing JSON!");
			}
			else if ($this->successor != NULL)
			{
				$this->successor->handleRequest($request);
			}
		}
		
	}

	class XMLService implements Handler
	{

		private $successor;
	 
		public function setSuccessor($nextService)
		{
			$this->successor = $nextService;
		}
	 
		public function handleRequest($request)
		{
			if ($request->getService() == "XML")
			{
				echo ("I am parsing XML");
			}
			else if ($this->successor != NULL)
			{
				$this->successor->handleRequest($request);
			}
		}
		
	}

We can now define some kind of request:

	class Request
	{

		private $value;
	 
		public function __construct($service)
		{
			$this->value = $service;
		}
	 
		public function getService()
		{
			return $this->value;
		
		}
		
	}

And our _Client_ could be like so:

	$json = new JsonService();
	$xml = new XMLService();

	$json->setSuccessor($xml);

	$request = new Request("XML");

	$json->handleRequest($request);

## Conclusion ##

Through this article, we saw how works the _Chain of Responsibility Pattern_. We saw that it is a _Behavioural Pattern_ that allows us to build a chain of objects to handle a call in sequential order. To apply this pattern, we need a _Handler_, a _Request Handler_ and a _Client_.

## One last word ##

If you like this article, you can consider supporting and helping me on [Patreon](https://www.patreon.com/mlbors)! It would be awesome! Otherwise, you can find my other posts on [Medium](https://medium.com/@mlbors) and [Tumblr](https://mlbors.tumblr.com/). You will also know more about myself on [my personal website](https://www.mlbors.com). Until next time, happy headache!