## Chapter 10: Sorting
* Categorized by:
  * Number of comparisons
  * Number of swaps
  * Memory usage
  * Recursion
  * Stability (are items with the same sort key value in the same order in the result?)
  * Adaptability (how does pre-sortedness affect performance)
* For sorts that use comparisons
  * Best case is O(n log n)
  * Worse case is O(n^2)

### Bubble Sort
* O(n^2) in best or worst case
* Smaller elements bubble to the top
```
def bubble_sort(array):
  for iteration in range(len(array)-1, -1, -1):
    for index in range(iteration-1):
      if array[index] > array[index+1]:
        array[index], array[index+1] = array[index+1], array[index]
```
* O(n) in improved version for an already sorted array
```
def bubble_sort_improved(array):
  swapped = True
  for iteration in range(len(array)-1, -1, -1):
    if not swapped:
      break
    swapped = False
    for index in range(iteration-1):
      if array[index] > array[index+1]:
        array[index], array[index+1] = array[index+1], array[index]
        swapped = True
```

### Selection Sort
* In-place sorting algorithm
* Repeatedly selects the smallest element
* Algorithm
  * Find the minimum value in the list
  * Swap it with the value in the current position
  * Repeat for all elements until entire array is sorted
* O(n^2)
```
def selection_sort(array):
  for i in range(len(array)):
    min = i
    for j in range(i+1, len(array)):
      if array[j] < array[min]:
        min = j
    array[min], array[i] = array[i], array[min]
```

### Insertion Sort
* Simple and efficient comparison sort
* Each iteration removes an element from the input data and inserts it into the correct position in the list being sorted
* Stable, in-place, adaptive
* O(n^2) but practically more efficient than selection sort or bubble sort
* Algorithm
  * Remove an element from the input data
  * Insert it into the correct position in the already sorted list until no input elements remain
  * This can be done in place
```
def insertion_sort(array):
  for i in range(1, len(array)):
    # assign next current element to insert into sorted portion of list
    curr = array[i]
    j = i

    # look at where in sorted list current element belongs
    while array[j-1] > curr and j >= 1:
      array[j] = array[j-1]
      j -= 1
    array[j] = curr
```

### Shell Sort
* More complex generalization of insertion sort
* Improves upon insertion sort by generating and sorting subarrays until the gap size is 1
* Makes several passes of the array, for a decreasing gap size
  * In each pass, sort the elements in the (i+gap*0, i+gap*1, i+gap*2, i+gap*3, …) subarrays
  * Decrease gap size, eventually reaching 1, which is one pass of insertion sort
```
def shell_sort(array):
  gaps = [701, 301, 132, 57, 23, 10, 4, 1]

  # Start with the largest gap and work down to a gap of 1
  for gap in gaps:
      # Do a gapped insertion sort for this gap size.
      # The first gap elements array[0..gap-1] are already in gapped order
      # keep adding one more element until the entire array is gap sorted
      for i in range(gap, len(array), 1):
          # add array[i] to the elements that have been gap sorted
          # save array[i] in temp and make a hole at position i
          temp = array[i]

          # shift earlier gap-sorted elements up until the correct location for array[i] is found
          j = i
          while j >= gap and array[j - gap] > temp:
              array[j] = array[j - gap]
              j -= gap

          # put temp (the original array[i]) in its correct location
          array[j] = temp
```

### Merge Sort
* MergeSort each half
* Merge the two halves
  * Two finger algorithm
  * O(n)
* O(n log n)
```
def merge_sort(array, low, high):
  if high <= low:
    return

  mid = (low + high)/2
  merge_sort(array, low, mid)
  merge_sort(array, mid+1, high)
  merge(array, low, high)

def merge(array, low, high):
  mid = (low + high)/2
  left = low
  right = mid+1

  sorted_array = []
  while left <= mid and right <= high:
    if array[left] <= array[right]:
      sorted_array.append(array[left])
      left += 1
    else:
      sorted_array.append(array[right])
      right += 1

  while left <= mid:
    sorted_array.append(array[left])
    left += 1

  while right <= high:
    sorted_array.append(array[right])
    right += 1

  for i in range(len(sorted_array)):
    array[low] = sorted_array[i]
    low += 1
```

### Quick Sort
* Divides a large array into two smaller subarrays: low elements and high elements
* Algorithm:
  * Pick an element (right-most or median of 3 random elements)
  * Partition
    * Reorder the array so that all the elements with values < pivot are before pivot
    * All elements with values > pivot come after pivot
  * Recursively apply the previous steps to the low and high subarrays
* Theta(n log n)
* O(n^2) when array is sorted or all the same element

### Linear Time Sorting
* Assume n keys are integers {0, 1, …, K-1} and each fits in a **word**
* Can do a lot more than comparisons
* For small enough k, can sort in O(n) time

#### Counting Sort
* Including assumptions above
  * Integer inputs
  * Relatively small k
* O(n+k)
```
L = array of k empty lists
for j in range(n):
  L[key(A[j])].append(A[j])

output = []
for i in range(k):
  output.extend(L[i])
```

### Radix Sort
* Imagine each integer in the input as base b
* # digits = d = log_b k
* Algorithm:
  * Sort integers by least significant digit
  * Sort integers by next least significant digit
  * …
  * Sort integers by most significant digit
* Each of the above sorts by digit uses counting sort
* Total time = O((n+b)*d) = O((n+b) * log_b k)
  * For b = Theta(n) ==> O(n log_n k)
  * When k <= n^c ==> **O(nc)**
