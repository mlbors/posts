# A word about the insertion sort

_Insertion sort_ is a simple sorting algorithm that builds the final sorted array or list one item at a time.

_The insertion_ sort is relatively efficient for small lists and works as follows:

That’s why we called this algorithm “insertion sort”: we take a number from the pile and insert it in the array in its proper sorted position.

A concrete example could be when people manually sort cards in a bridge hand, most use a method that is similar to insertion sort.

Down below, you will find the pseudo-code of the _insertion sort_ algorithm:
    
    
    for i = 1 to length(Array)
        temp = Array[i]
        j = i - 1
        while j >= 0 and Array[j] > temp
            Array[j+1] = A[j]
            j = j - 1
        end while
        Array[j+1] = temp[4]
     end for

Let’s try a concrete implementation with _Ruby_:
    
    
    module InsertionSort
      def self.sort(array)
        1.upto(array.size - 1) do |i|
          j = i
          while j > 0 && array[j] < array[j - 1]
            array[j], array[j - 1] = array[j - 1], array[j]
            j -= 1
          end
        end
    
        return array
    
      end
    end

**Runtime:**

  * Average: O(N)
  * Worst: O(N^2)


