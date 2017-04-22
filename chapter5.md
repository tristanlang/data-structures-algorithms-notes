## Chapter 5: Queues
* FIFO data structure
  * Ordered list in which insertion occurs at the rear and deletion occurs at the front
  * Enqueue at the rear
  * Dequeue from the front
  * Underflow = dequeue an empty queue
  * Overflow = enqueue onto a full queue
* Queue ADT
  * Enqueue
  * Dequeue
  * Front
  * QueueSize
  * IsEmpty
* Implementation
  * Simple circular array
    * Treat last element and the first array elements as contiguous
    * Use two variables to keep track of indexes for front and rear
    * Enqueuing flows around the array if at the end of the array
    * O(1) for all functions
    * O(n) for space for array
  * Dynamic circular array
  * Linked list
    * Use pointers for front, rear (head, tail, respectively)
    * O(1) for all operations
