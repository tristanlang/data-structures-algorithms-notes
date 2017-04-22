## Chapter 12: Selection Algorithms 
* Find the k-th largest number in a sequence 
* Selection by sorting
  * Sort list then select desired element
  * Advantageous if selecting many elements
  * O(n log n) to sort
  * Amortized to O(log n) if selecting n elements 
* Finding the k-th smallest element in sorted order
  * Balanced tree method
    * Put first k elements into balanced tree
    * For each remaining element
      * if > largest in tree, continue
      * else, remove largest in tree and replace with current element = O(log k)
    * k-th smallest is largest in tree
  * **Partition method (similar to quicksort - a.k.a quickselect)**
    * Choose a pivot element and partition and sort around it
    * if partition index < k, k-th smallest is in right side so partition that side
    * else if partition index == k, return element
    * else, partition index > k, so partition left side
    * O(n^2) worst case
    * Theta(n log k) on average ==> linear in n
