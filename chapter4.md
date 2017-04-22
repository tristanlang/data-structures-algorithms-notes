## Chapter 4: Stacks
* LIFO data structure
  * Ordered list in which insertion and deletion occur on one end, called the top
  * Push onto the top
  * Pop off of the top
  * Underflow = pop off an empty stack
  * Overflow = push onto a full stack
* Stack ADT
  * Push
  * Pop
  * Top (retrieve top element)
  * Size
  * IsFull/IsEmpty
* Implementation
  * Simple array
    * O(1) for all operations
    * Maintain an index for the top of the stack
    * Access the top element with this index
  * Dynamic array
    * Similar to the fixed array implementation
    * Double the array size every time the array is full
    * O(1) for all operations except when the array needs to be doubled and copied, which is O(n)
    * Too many doublings may cause memory overflow
  * Linked list
    * Push onto stack by inserting at head of list
    * Pop off stack by removing from head of list
    * O(1) for all operations except delete stack (O(n))
* Tradeoff is the expensive doubling operation for arrays when they need to resize vs extra space needed for references
* Stack to convert infix to postfix notation (basic shunting yard algorithm)
  * 5*(2+3-4) -> 5 2 3 4 - + *
  * Loop through input characters 
  * If char is a number, output it
  * If char is an operator, pop off stack and output operator until empty or lower priority operator is encountered, push onto the stack
  * If char is ( push onto stack
  * If char is ) pop off stack and output until reaching (
  * When reaching end of input, pop off stack and output
* Stack to evaluate postfix notation
  * 1 2 3 + - 1 * -> (1-2+3)*1
  * Push numbers onto the stack
  * When encountering a binary operator, pop two numbers off the stack, apply the operator, push result onto stack
  * When encountering a unary operator, pop one number off the stack, apply the operator, push result onto stack
* Shunting yard algorithm
  * Can be used to convert from infix to postfix
  * While there are tokens to be read:
    * Read a token
    * If the token is a number, push it to the output queue
    * If the token is a function token, push it onto the operator stack
    * If the token is a function argument separator (comma)
      * Until the token at the top of the operator stack is a left parenthesis, pop operators off the operator stack onto the output queue
      * If no left parentheses are encountered, either the separator was misplaced or parentheses were mismatched    
    * If the token is an operator, op1, then:
      * while there is an operator token, op2, at the top of the operator stack and either:
        * op1 is left-associative and its precedence is <= that of op2
        * op1 is right-associative and its precedence is < that of op2
        * pop op2 off the operator stack and onto the output queue
      * at the end of the iteration, push op1 onto the operator stack
    * If the token is a left parenthesis, push it onto the operator stack
    * If the token is a right parenthesis:
      * Until the token at the top of the stack is a left parenthesis, pop operators off the operator stack onto the output queue
      * Pop the left parenthesis from the operator stack, but not onto the output queue
      * If the token at the top of the operator stack is a function token, pop it onto the output queue
      * If the operator stack runs out without finding a left parenthesis, then there are mismatched parentheses
  * When there are no more tokens to be read:
    * While there are still operator tokens in the stack:
      * If the operator token on the top of the stack is a parenthesis, then there are mismatched parentheses
      * Pop the operator onto the output queue
  * Exit
