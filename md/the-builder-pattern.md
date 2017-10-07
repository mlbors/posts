In this post we are going to see what the _Builder Pattern_ is. Let's get into it!

The _Builder Pattern_ is a _Creational Pattern_ that offers a solution to the _telescoping constructor anti-pattern_. It allows us to create different "_flavors_" of an object without polluting the _constructor_. 

This pattern is useful when the construction process must allow different representations of an object or when the creation of an object involved a lot of steps.

The main difference from the _Factory Pattern_ is that this last one should be used when the object creation is a one step-process.

Let's make a quick example written with _PHP_ and just for fun, let's say we need to implement the _Builder Pattern_ to manage our hero in our brand-new video game.

First, we define a _Builder Interface_:

	interface BuilderInterface
	{
		public function addHair();
		public function addArmor();
		public function addWeapon();
		public function build();
	}

Then we can implement our interface:

	class HeroBuilder implements BuilderInterface
	{
		public $name;

		public $hair;
		public $armor;
		public $weapon;
		
		public function __construct($name)
		{
			$this->name = $name;
		}

		public function addHair()
		{
			$this->hair = "Hero Hair";
		}
		
		public function addArmor()
		{
			$this->armor = "Hero Armor";
		}
		
		public function addWeapon()
		{
			$this->weapon = "Hero Weapon";
		}
		
		public function build()
		{
			return new Hero($this);
		}
	}

Finally we create a _class_ for our hero:

	class Hero
	{
		protected $name;

		protected $hair;
		protected $armor;
		protected $weapon;
		
		public function __construct($builder)
		{
			$this->name = $builder->name;
			$this->hair = $builder->hair;
			$this->armor = $builder->armor;
			$this->weapon = $builder->weapon;
		}
		
	}

And we create our hero like so:

	$hero = (new HeroBuilder("The Hero"))
				->addHair()
				->addArmor()
				->addWeapon()
				->build();