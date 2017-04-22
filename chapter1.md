## Chapter 1: Introduction
* 1 byte = 8 bits (00000000 through 11111111)
* Data structure = special format for organizing and storing data
  * Linear data structures: elements accessed in a sequential order
  * Nonlinear data structures: elements stored/accessed in a nonlinear manner (trees/graphs)
* Abstract Data Types (ADTs) = combination of declaration of data structures and operations that can be performed on them
  * When defining ADTs don't worry about implementation details
* Algorithm = step by step instructions to solve a given problem
* Types of algorithmic analysis
  * Worst case
  * Best case
  * Average case
* Big-O Notation = Asymptotic tight upper bound on an algorithm's performance for inputs of large size
* Omega Notation = Tighter lower bound of an algorithm's performance for inputs of large size
* Theta Notation = average case of an algorithm's performance (an average of all the complexities of the algorithm)
  * Represents the upper and lower bounds for the bounds of an algorithm's performance for large inputs
  * Used especially if upper and lower bounds are the same
* Recurrence problems
  * A recurrence is when an algorithm divides itself into smaller components plus a set amount of work per component
  * Guess the big-o, omega, or theta Notation
  * Prove it correct by induction (or incorrect and guess again)
* Amortized analysis = worst case analysis for a sequence of operations rather than for individual operations
  * The operation in the sequence with the worst performance will be amortized over the entire sequence
