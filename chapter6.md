## Chapter 6: Trees
* Hierarchical data structure
  * Each node points to a number of nodes, not just one
  * Nonlinear
  * Order of elements is not important
* Root node has no parents
* Edge is the link from parent to child
* Leaf node has no children
* Sibling nodes have the same parent
* Ancestor/descendant
* Level refers to the set of nodes at a given depth
* Height of node is length of path from that node to the deepest node
* Depth of node is length of path from root to node
* Height of tree is length of path from root to deepest node
* Skew tree is one in which every node has one child

### 6.4 Binary tree
* General terminology and classification
  * 0, 1, 2 children
  * Left, right subtrees
  * Strict binary tree is one in which each node has two children or no children
  * Full binary tree is one in which each node has exactly two children and all leaf nodes are st the same level
  * Complete binary tree is one in which all levels, except possibly the last, are full and leaf nodes are as far left as possible
* Properties of Binary Trees
  * 2^height = number of nodes in a level of a binary tree
  * 2^(height+1)-1 = number of nodes in a full binary tree for a full tree of a given height
  * The number of null pointers in a binary tree with n nodes is n+1
* Binary Tree ADT
  * Data element
  * Left pointer
  * Right pointer
  * Insert
  * Delete
    * Find node to delete
    * Find deepest node (last node in level order traversal)
    * Replace node to delete with deepest node
    * Delete deepest node
  * Search
  * Traverse
  * Size
  * Height = longest path from node down to a leaf
  * Lowest common ancestor
* Binary Tree Traversals
  * Each node may be visited multiple times but us only processed once
  * Like searching the tree except that in traversal the goal is to move through the tree in a particular order
* Pre-Order
  * Each node is processed before subtrees
  * Must keep track of processed nodes that must be returned to, so stack can be used to preserve LIFO
* In-Order
  * Root is visited between the subtrees
  * Use stack again to keep track of nodes that need to be processed
* Post-Order
  * Node is visited after the subtrees
  * Use stack again to keep track of nodes that need to be processed
  * Also keep track of previous node to know where in traversal process we are
    * If previous is left child, we are returning from processing left subtree, so push node and process right subtree
    * If previous is right child, we are returning from processing right subtree, so print current node
* Level Order
  * Visit root
  * While traversing level L, keep all elements at level L+1 in queue
  * Go to next level and visit all nodes at that level
  * Use null to indicate the progression between levels
  * Repeat until all levels are completed

### 6.5 Generic trees (N-ary trees)
* Representation (first child/next sibling)
  * At each node, link children of same parent (siblings) from left to right
  * Remove the links from parent to all children except the first child
* In practice this is a binary tree with different names for left and right

### 6.6 Threaded (Stack or Queue less) Binary Tree Traversals
* Stack/queue representation will waste storage space due to majority null pointers
* Rather than wasting pointers to null, store predecessor/successor information to avoid stack/queue
  * Predecessor/successor pointers are called threads
  * Left threaded = only use left pointers for predecessor information
  * Right threaded = only use right pointers for successor information
  * Fully threaded = left pointers for predecessors and right pointers for successors
* **Successor/predecessor information is based solely on whether tree type is preorder, inorder, or postorder**
* Data structure modifications
  * LTag and RTag to indicate whether the left or right pointer points to child (tag = 1) or successor/predecessor (tag = 0)
  * Always included is a dummy node which indicates which node is the root of the tree and to which used successor or predecessor pointers can point
  * A pointer would be on used if a node has no child and no successor or no predecessor
* Finding inorder successor inorder threaded binary tree
  * If current node has no right subtree, return the right child (which points to the successor)
  * If current node does have a right subtree, go as far as possible to the left of the right child of the current node
* Finding pre-order successor of inorder threaded binary tree
  * If current node has a left subtree, return left child
  * If current node does not have a left subtree, traverse right child (inorder successor) pointers until the first true right child is reached
* Insertion of node in inorder threaded binary tree
  * Node to which to append to has no child
  * Node to which to append to has a child

### 6.9 Binary search trees (BSTs)
* All nodes in the left subtree contain values less than the current node
* All nodes in the right subtree contain values is greater than the current node
* Also subtrees must also be binary search trees
* O(log n) search in a balanced tree
* O(n) search in a skew tree
* Insert
  * Attempt to find the element in the tree
  * If not found, insert in the last location of the path traversed
  * **Node is not inserted between other nodes, but rather simply added as a leaf of an existing node**
* Delete
  * Find node to be deleted
  * If node has no children, delete it and set parent child to null
  * If node has one child, delete node and set its child to the child of its parent
  * If node has both children, replace node with largest element in its left subtree and recursively delete that largest element
* **Lowest common ancestor (LCA) is lowest node that connects two nodes in the tree**
  * O(n) algorithm
    * To find LCA of two nodes, traverse from root to bottom in preorder
    * Then, the first node whose value is between the two target values is the LCA
  * Theta(log n) algorithm (for balanced tree)
    * Traverse from root down
    * while True
    * if node value is lower than both targets, set node to node.right
    * if node value is greater than both targets, set node to node.left
    * otherwise, node is within targets, return node

### 6.10 Balanced BSTs
* Impose restrictions on tree height to avoid O(n) complexity of skew trees
* HB(k) ==> height balance, k is left height - right height

### 6.11 AVL (Adelson-Velskii and Landis) Trees
* HB(k), where k = 1
* BST where left and right subtree heights differ by at most 1
* Minimum/Maximum number of nodes in AVL tree
  * Minimum heigh ~ log n
    * N(h) = N(h-1) + N(h-2) + 1
    * Number of nodes at height h = Number in subtree with height h-1 + Number in subtree with height h-2 + root
  * Maximum height ~ log n
* AVL Tree declaration includes height parameter for simplicity of operations
* Rotations
  * Preserve AVL property when heights differ by 2 (assuming tree was balanced every time balance was violated)
  * After an insertion, only nodes that are on the path from the insertion point to the root might have their balances altered
  * **TO RESTORE AVL TREE PROPERTY, start at insertion point and keep going to the root of the tree**
  * While moving to the root, need to consider the first node that does not satisfy the AVL property
  * **From that node onwards, every node on the path to the root will have the issue**
  * Always need to care for the first node that is not satisfying the AVL property on the path from the insertion point to the root and fix it
* Types of Violations
  * Suppose X must be rebalanced and it is the first node whose subtrees differ by height 2
  * Violation may have occurred in one of 4 ways
    1. Insertion into left subtree of left child of X (solve with LL rotation)
    2. Insertion into right subtree of left child of X (solve with LR rotation)
    3. Insertion into left subtree of right child of X (solve with RL rotation)
    4. Insertion into right subtree of right child of X (solve with RR rotation)
* Single Rotations
  * LL Rotation
    * X is node to be rebalanced
    * create new node W
    * set X’s R child to its L child’s R child
    * set X as the R child of its L child
    * update heights
    * return pointer to W for X’s former parent to point to
  * RR Rotation
    * mirror of LL
* Double Rotations
  * Perform two single rotations (one left and one right)
* Insertion into AVL Tree
  * Insert as with any BST
  * Check for imbalance
  * Rotate if necessary
* AVL Sort
  * insert n items in AVL tree = O(n log n)
  * inorder traversal to output = O(n)
