## [The Bubble sort](https://mlbors.tumblr.com/post/159183744440/the-bubble-sort)

Let’s discover rapidly what is _Bubble sort_!

_Bubble sort_ is a simple sorting algorithm that works by repeatedly stepping through a list to be sorted, comparing each pair of adjacent items and swapping them if the first element is greater than the second element. The pass through the list is repeated until no swaps are needed, which indicates that the list is sorted. The algorithm is named for the way smaller or larger elements “_bubble_” to the top of the list.

**Runtime:**

  * Average: O(N^2)
  * Worst: O(N^2)



**Memory:**

  * O(1)



Down below, a simple implementation of _Bubble sort_ made with _Python_:
    
    
    def bubble_sort(array):
      i = int(len(array) - 1)
      while i >= 1:
        for j in range(0, i):
          if array[j] > array[j + 1]:
             swap_values(array, j, j + 1)
        i -= 1
      return array
    
