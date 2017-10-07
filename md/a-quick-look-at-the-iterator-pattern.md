# A quick look at the Iterator Pattern #

In this post, we are going to have a quick look at the _Iterator Pattern_.

The _Iterator Pattern_ is a _Behavioral Pattern_ that presents a way to access the elements of an object without exposing the underlying presentation. In other words, it offers a solution to iterate through a container and access to the elements stored in that last one.

A very common real world example is an old radio set, where a user can start at some channel and then use the "_next_" button or the "_previous_" button to go through the different channels.

Let's make a simple example with _PHP_ using the _Iterator_ and the _Countable_ interfaces. Those interfaces are available in the _Standard PHP Library_ (SPL). We are going to base our implementation on our radio set example.

The _Iterator interface_ is defined as so:

	Iterator extends Traversable {
		abstract public mixed current ( void )
		abstract public scalar key ( void )
		abstract public void next ( void )
		abstract public void rewind ( void )
		abstract public boolean valid ( void )
	}

The _Countable interface_ looks like so:

	Countable {
		abstract public int count ( void )
	}

Let's start by defining a _RadioStation class_:

	class RadioStation
	{
		protected $frequency;

		public function __construct(float $frequency)
		{
			$this->frequency = $frequency;
		}

		public function getFrequency(): float
		{
			return $this->frequency;
		}
	}

Now we can build our _Iterator_ the following way:

	use Countable;
	use Iterator;

	class StationList implements Countable, Iterator
	{

		protected $stations = [];
		protected $counter;

		public function addStation(RadioStation $station)
		{
			$this->stations[] = $station;
		}

		public function removeStation(RadioStation $toRemove)
		{
			$toRemoveFrequency = $toRemove->getFrequency();
			$this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
				return $station->getFrequency() !== $toRemoveFrequency;
			});
		}

		public function count(): int
		{
			return count($this->stations);
		}

		public function current(): RadioStation
		{
			return $this->stations[$this->counter];
		}

		public function key()
		{
			return $this->counter;
		}

		public function next()
		{
			$this->counter++;
		}

		public function rewind()
		{
			$this->counter = 0;
		}

		public function valid(): bool
		{
			return isset($this->stations[$this->counter]);
		}
	}

Notice that we use the _array_filter_ function with two arguments in the _removeStation method_. The first argument specifies the array to filter and the second is the _callback_ function, here used as an _anonymous function_ that we rely on to filter the array.

Then it can be used like so:

	$stationList = new StationList();
	$stationList->addStation(new RadioStation(87));
	$stationList->addStation(new RadioStation(103));
	$stationList->addStation(new RadioStation(106.5));

	foreach($stationList as $station) {
		echo $station->getFrequency();
	}

	$stationList->removeStation(new RadioStation(87));