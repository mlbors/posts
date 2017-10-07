# A word about the Strategy pattern

_The Strategy pattern_ is a behavioural design pattern. Let’s talk a bit about it!

_The Strategy pattern_ allows you to switch the algorithm or strategy based upon the situation. The pattern defines a family of algorithms, encapsulate each one, and make them interchangeable and lets the algorithm vary independently from clients that use it.

Down below, a simple implementation of the Strategy pattern made with _PHP_:

First, a comparator interface and two concrete implementations.
    
    
    interface ComparatorInterface
    {
      public function compare($a, $b): int;
    }
    
    
    
    class DateComparator implements ComparatorInterface
    {
      public function compare($a, $b): int
      {
        $aDate = new \DateTime($a['date']);
        $bDate = new \DateTime($b['date']);
        return $aDate <=> $bDate;
      }
    }
    
    
    
    class IdComparator implements ComparatorInterface
    {
      public function compare($a, $b): int
      {
        return $a['id'] <=> $b['id'];
      }
    }
    

…and an object comparator
    
    
    class ObjectComparator
    {
      private $elements;
      private $comparator;
    
      public function __construct(array $elements = [])
      {
        $this->elements = $elements;
      }
    
      public function sort(): array
      {
        if (!$this->comparator) {
          throw new \LogicException('Comparator is not set');
        }
            
        uasort($this->elements, [$this->comparator, 'compare']);
        return $this->elements;
      }
    
      public function setComparator(ComparatorInterface $comparator)
      {
        $this->comparator = $comparator;
      }
    }

…and in use!
    
    
    $data = [['date' => '2014-03-03'], ['date' => '2015-03-02'], ['date' => '2013-03-01']];
    $obj = new ObjectComparator($data);
    $obj->setComparator(new DateComparator());
    $elements = $obj->sort();
    
    $data = [['id' => 2], ['id' => 1], ['id' => 3]]
    $obj = new ObjectComparator($data);
    $obj->setComparator(new IdComparator());
    $elements = $obj->sort();
    
