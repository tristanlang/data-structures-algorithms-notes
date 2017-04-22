## Chapter 3: Linked Lists
* Data structure for storing collections of data, such that successive elements are connected by pointers with last element pointing to null
* Can grow and shrink in size during execution and does not waste memory
* Primary operations for the linked list ADT
  * Insert
  * Delete (and return element at specified position)
  * Delete list
  * Count
  * Find n-th
* Arrays
  * One memory block is allocated for an entire array
  * Array elements can be accessed in constant time
    * The size of the data type is multiplied by the index and added to the memory address of the array itself
    * The result is the memory address of the desired element, after one multiplication and one addition operation
  * Dynamic arrays can make it easy to add and remove elements
* Linked lists vs arrays
  * Arrays are simpler and have faster access to the elements
  * But they require a single block of memory and inserting an element requires shifting elements after it
  * Linked lists can be expanded in constant time
  * But access time for linked lists is linear in the worst case, they require extra manipulation when deleting the last item, and they require additional memory for reference pointers
* Doubly linked list
  * Pointer to previous node and next node
  * Convenience at the expense of more space used and more operations required when inserting or deleting
* Circular Linked List
  * Each node has a successor
  * Only node needed for access is the tail node (which provides access to the head node)
  * Used in managing the computing resources of a computer
  * Can be used to implement stacks and queues
* Unrolled Linked Lists
  * Store a circular linked list in the element of each node
  * All nodes should contain no more than sqrt(n) elements, except the last one (n elements in the full list)
  * When inserting an element, elements may need to be moved to the next circular linked list
  * This shift operation requires removing a node from the tail (head.next) of the CLL in a block and inserting it to the head of the next CLL
    * O(sqrt(n)) because there are O(sqrt(n)) blocks and each insertion is O(1)
  * Can save a significant amount of memory as compared with doubly linked lists
* Skip Lists
  * Linked list with additional pointers to skip intermediate nodes 
  * Can be used as an alternative to balanced binary trees
    * Skip list allows for a quick search, insertion, and deletion of elements
  * When a node is generated, it is randomly determined how many forward pointers it will have
  * Can have as many levels of forward pointers as desired, but cap them at MaxLevel
  * O(log n) performance for search if forward pointers are distributed in a base 2 manner (and only 2x the number o pointers)
  * O(log n) performance for insert and delete operations
* The sorting technique
  * List pointers in array and sort them
  * Duplicates will be next to one another
* The search technique
  * List all pointers in one array
  * Use a hash table to traverse the array and find the first repeat
* Floyd cycle finding algorithm
  * Use two pointers to identify whether a linked list is a cycle or whether it terminates
  * Slow pointer moves one node at a time
  * Fast pointer moves two nodes at a time
  * If they meet, there is a cycle
  * If one hits null, the list terminates
  * **To find the start of the cycle**
    * Once finding where the fast and slow pointers meet, slow will have moved s+x nodes and fast will have moved 2s+2x (s is distance to start of loop, x is some number of nodes traversed in the loop)
    * Move the slow pointer back to the head
    * Advance each one node at a time now
    * When they meet again, they will be at the start of the cycle
    * This works because the formerly fast pointer moved an additional distance s+x in the loop to arrive at position x, and advancing it by only s (s+x-x), it will be at the start of the loop
* Reversing a list
  * Set current to head
  * While curr.next isn't null
    * Set next to curr.next
    * set curr.next to prev
    * set prev to curr 
    * set curr to next
