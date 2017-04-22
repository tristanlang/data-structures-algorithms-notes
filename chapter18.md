## Chapter 18: Divide and Conquer Algorithms
* Recursively break down a problem into 2+ subproblems of the same type, until they become simple enough to be solved directly, and then combine solutions to subproblems to get solution to original problem
* Sometimes useful when Greedy fails
* Subproblems must be smaller instances of the same type of problem as the original problem

### Advantages of Divide and Conquer
* Solving difficult problems (towers of Hanoi)
* Parallelism
* Memory access: once a subproblem is small, it can be solved within the cache rather than in slower main memory

### Disadvantages of Divide and Conquer
* Recursion is slow
* May be more complicated than iterative approaches

### Applications
* Binary search
* Merge sort and quick sort
* Median finding
* Min and max finding
* Matrix multiplication
* Closest pair problem
