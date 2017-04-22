## Chapter 7: Priority Queues and Heaps

### Priority Queue
* Queue data structure in which dequeuing order is based on element value
  * Ascending priority queue always dequeues smallest element (smallest has highest priority)
  * Descending priority queue always dequeues largest element (largest has highest priority)
* ADT
  * Insert (enqueue)
  * DeleteMin
  * DeleteMax
  * GetMin/GetMax
* Can be represented with list, array, tree data structures but ideal is min/max heap

### Heap
* O(log n) insert/delete and O(1) get min/max
* Heap is a tree data structure with special properties
  * Heap property = each node is >= its children (for max heap) or <= its children (for min heap)
  * MUST be a complete tree (nodes are at levels h or h-1)
* Heap ADT
  * array (elements in heap)
  * Count (number of elements in heap)
  * Capacity (of heap/array)
  * Heap type (min/max)
  * GetParent
    * For a node at location i, its parent is at (i-1)/2
  * GetLeft/GetRight
    * For a node at location i, its children are at 2i+1 and 2i+2
  * GetMax/GetMin = first element in the array, root of tree
* Heapifying
  * After inserting an element, the heap property may be violated
  * To identify, check from root down to inserted element whether heap property has been violated
  * If it has been, swap node with the greater of its children
  * Repeat this process down until heap property is not violated
  * This is PercolateDown
  * Can also PercolateUp from a non root node up to the root
  * O(log n) because it is based on the height of the nearly balanced tree
* Delete element
  * Only root is deleted because of heap
  * Store root in temp var to return
  * Replace last element with root
  * Delete last element
  * Percolate new root down
  * Decrease heap size
  * Return temp var
  * O(log n)
* Insert element
  * Add to end of heap
  * Percolate up
  * Increase heap size
  * O(log n)
* Heapsort
  * Add unsorted array to heap
  * Build max heap (heapify) O(n)
  * Swap root with last element of heap O(1)
  * Decrease heap size
  * Heapify root O(log n)
  * Repeat until done with all elements
  * Array is sorted in place by performing n heapify operations plus build heap from unsorted array = O(n) + O(n log n) ~ O(n log n)
* Build heap from unsorted array
  * for (n-1)/2 down to 0: heapify node
  * No need to heapify beyond element (n-1)/2 because those have no children and are already heaps
  * O(n)
    * Only root node performs log n operations
    * Nodes one level above leaves perform O(1) operation, and there are n/4 of them
    * The whole series is then n times a series which converges to a constant so it is linear (sum of inverse powers of 2)
