Overview of Dependency Injection

Through this article, we are going to overview what _Dependency Injection_ is and how it works.

_Dependency Injection_, also called _DI_, is a _design pattern_ in which one or more dependencies (_services_) are injected into a dependent object (_client_). This pattern allows us to implement a loosely coupled architecture by separating the creation of a client's dependencies from its own behavior.

We can apply this pattern when we want to remove knowledge of concrete implementations from objects, but also when we want to get a better testable code in isolation using mock objects.

The _DI_ pattern is commonly used to implement the _Inversion of Controle Principle_, which in a few words, separates the _what-to-do_ part of the _when-to-do_ part. In other words, it is about letting somebody else handles the flow of control. It is based on the _Hollywood Principle_: "Don't call us, we'll call you".

## Basic examples ##
The _DI_ pattern can be implemented in several ways. Let's take a look at some examples written in _PHP_.

### Constructor Injection ###
Here, we will inject dependencies using a _constructor_. We can imagine a really simple example like so:

	interface ITire()
	{
		public function spin();
	}

	interface IEngine()
	{
		public function run();
	}

	class TireImpl1 implements ITire
	{
		public function spin()
		{
			echo 'Tire 1 is spinning!';
		}
	}

	class TireImpl2 implements ITire
	{
		public function spin()
		{
			echo 'Tire 2 is spinning!';
		}
	}

	class EngineImpl1 implements IEngine
	{
		public function run()
		{
			echo 'Engine 1 is running!';
		}
	}

	class EngineImpl2 implements IEngine
	{
		public function run()
		{
			echo 'Engine 2 is running!';
		}
	}

	class Car()
	{

		private $engine;
		private $tire;
		
		public function __construct(IEngine $engine, ITire $tire)
		{
			$this->engine = $engine;
			$this->tire = $tire;
		}
		
		public function run()
		{
			$this->engine->run();
			$this->tire->spin();
			echo 'Car is running'.
		}

	}

	$car = new Car(new EngineImpl1(), new TireImpl1());

In the above example, we see that our _Car_ object is not tightly coupled with its dependencies which are not hard-coded and each class can be used independently. By using interfaces, we can pass any object to the _Car's constructor_ if this object implements the right interface. Even if the _Car_ class needs an _Engine_ and a _Tire_, it is not its role to create them. The less _Car_ knows about how work its dependencies, the better it is. We can also see that our _Car_ class can be easily tested because its dependencies can be mocked.

_Constructor injection_ is the most common method used for _Dependency Injection_. The advantages are that the dependencies can't be changed during the object lifetime and the required dependencies are present when the object gets instantiated.

### Setter Injection ###
Another common way to use _Dependency Injection_ is the _Setter Injection_. To use this method, we can modify our previous example like so:

	class Car()
	{

		private $engine;
		private $tire;
		
		public function __construct()
		{

		}
		
		public function setEngine(IEngine $engine)
		{
			$this->engine = engine;
		}
		
		public function setTire(ITire $tire)
		{
			$this->tire = tire;
		}
		
		public function run()
		{
			$this->engine->run();
			$this->tire->spin();
			echo 'Car is running'.
		}

	}

The _Setter Injection_ allows us to have optional dependencies with default values, to add new dependencies very easily and to call the _setters_ multiple times if needed.

### Container ###
A _Dependency Injection Container_ is an object that handles the instantiation of other objects.

The creation of _DIC_ is another subject that we will not see in this article. By the way, there are many popular _DIC_ like _Pimple_, _Dice_ or _PHP-DI_. The _Zend Framework_ and _Symfony_ also provide their own container.

One way to build a container could consist to create a file, like a _JSON_ file, where we would list our different dependencies and their arguments. Then we could create a _Container_ class where will use a _Repository_ object to store instantiated services and a _Factory_ object to create the services that have not been instantiated.

## DI and Autoloading ##
_DI_ could make us think to _Autoloading_ but they should not be confused. _Autoloading_ ensures that our classes are defined when we reference them. _Dependency Injection_ gives us flexibility over what instance of an object we use. _Autoloading_ knows how to load the classes, where each class is defined and includes the correct file. A _DIC_ knows how to build objects of various types. It doesn't know about class loading.

## DI and Strategy ##
_DI_ could also make us think about the _Strategy Pattern_. These patterns work the same way, but _Strategy_ is used for short-lived dependencies. With _DI_, it is most unusual that the dependencies of objects change during their lifetime, while this is not uncommon with _Strategy_. With _Strategy_, classes are designed so that they can be configured with an algorithm at runtime. With _DI_, classes get an algorithm injected at runtime.

## Using DI with Java ##
If we want to use _DI_ with _Java_, we can make it without any additional framework. By the way, _Spring_ and _Google Guice_ are two very popular frameworks.

However, we can use _annotations_ to define injection points using _CDI_, available in _Java EE_, _Google Guice_, or _Spring_ like so:

	public interface EngineService {
		void getService();
	}

	public class Impl1EngineService implements EngineService {

		@Override
		public void getService() {
			System.out.println("Impl1EngineService");
		}
		
	}

	// Injection through fields
	public class Car {

		@Inject
		@Named("Impl1EngineService")
		private EngineService Impl1EngineService;

	}

	// Injection through constructor
	public class Car {

		private EngineService Impl1EngineService;
		
		@Inject
		public Car(@Named("Impl1EngineService") EngineService Impl1EngineService) {
			this.Impl1EngineService = Impl1EngineService;
		}

	}

	// Injection through setter method
	public class Car {

		private EngineService Impl1EngineService;
		
		@Inject
		public void setImpl1EngineService(@Named("Impl1EngineService") EngineService Impl1EngineService) {
			this.Impl1EngineService = Impl1EngineService;
		}

	}

## A word about Angular ##
In _Angular_, _DI_ is widely used and we can take a moment to dig a little into it.

_Angular_ uses its own _Dependency Injection_ framework that basically uses three things:

* _The Injector_, that exposes APIs
* _The Provider_, that tells the injector how to create an instance of a dependency
* _The Dependency_, the type of which an object should be created

_Angular_ has a _Hierarchical Dependency Injection_ system. There is a tree of injectors that parallel an application's component tree. An application may have multiple injectors. That means we can configure providers at different levels:

* For the whole application when _bootstrapping_ it. All _sub injectors_ will see the _provider_ and share the instance associated with. It will always be the same instance.
* For a specific _component_ and its _sub components_. Other _components_ won't see the _provider_.
* For _services_. They use one of the _injectors_ from the element that calls the _service_ chain.

When using _DI_ with _Angular_, we will mainly see the following decorators:

* _@Injectable_, that marks a class as available to Injector for creation
* _@Inject_, which specifies a dependency

Knowing what we know about _DI_, we can quickly build something like this:

	import { Injectable } from '@angular/core'; 

	@Injectable() 
	   export class engineService {  
	   
	   getEngine(): string { 
		  return "Engine 1"; 
	   } 
	   
	}

	import { Component } from '@angular/core';  
	import { engineService } from './engine.service';  

	@Component({ 
	   selector: 'app', 
	   template: '<div>{{value}}</div>', 
	   providers: [engineService]  
	}) 

	export class AppComponent { 

	   value: string = ""; 
	   
	   constructor(private _engineService: engineService) { } 
	   
	   // Our, without TypeScript
	   // constructor(@Inject(engineService) private engineService) { }
	   
	   ngOnInit(): void { 
		  this.value = this._engineService.getEngine(); 
	   }  
	   
	}

## Conclusion ##
Through this article, we briefly saw what _Dependency Injection_ is and how it could be quickly implemented. We also saw that many frameworks or libraries exist to help us use _DI_ efficiently.