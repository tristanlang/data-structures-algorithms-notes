## Chapter 19: Dynamic Programming
* DP = recursion + memoization
* Useful when we fail to get optimal solutions with Greedy or divide and conquer
* Differs from divide and conquer because d&c subproblems are independent but DP subproblems may overlap
* Reduces exponential complexity to polynomial
* Properties of DP Strategy
  * Optimal substructure = an optimal solution contains optimal solutions to subproblems
  * Overlapping subproblems = a recursive solution contains a small number of distinct subproblems repeated many times
* Top down vs bottom up
* DP problems
  * String longest common subsequence, longest increasing subsequence, longest common substring
  * Efficient graph algorithms (Bellman-Ford)
  * Chain matrix multiplication
  * Subset sum
  * Knapsack
  * Traveling Salesman

### OCW Notes
* Useful for optimization problems (min/max)
* DP = Careful brute force
* DP = memoize and reuse solutions to subproblems that help solve the problem = **guessing + recursion + memoization**
* DP = shortest paths in some DAG
* **RUNNING TIME = ( # subproblems ) x ( time per subproblem )**
* Bottom up DP = topological sort of subproblem DAG
* Guess…try ALL guesses and take the best one
* **Subproblem dependencies should be acyclic**
* Five Steps to DP:
  1. Subproblems (**THINK ABOUT SUFFIXES**) ==> # subproblems
  2. Guess (part of solution) ==> # choices for guess
  3. Recurrence ==> time/subproblem
  4. Recurse and memoize OR build DP table bottom-up ==> check subproblem recurrence is acyclic (i.e., has topological order)
  5. Solve original problem ==> total time
* Parent pointer = remember which guess was best…follow parent pointers to get the optimal subproblem solutions that build the optimal solution

### Subproblems for Strings/Sequences
* Suffixes — x[i:] for all i
* Prefixes — x[:i] for all i
* All substrings — x[i:j] for all i <= j

### Knapsack
* List of items, each with size s_i and value v_i (both integers)
* Knapsack of size S
* maximize sum of values for a subset of items of total size <= S
  2. Guessing: is item i in subset or not?
  1. Subproblem = suffix `i:` of items & remaining capacity X <= S
  3. DP(i, X) = max( DP(i+1, X), DP(i+1, X-s_i) + v_i )
  4. Total time = O(nS) — pseudopolynomial time because S is one of the inputs

