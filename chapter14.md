## Chapter 14: Hashing
* Worst case = O(n)
* Average case = O(1)
* Hashing = mapping keys to memory locations
  * This is typically achieved by converting the key to a number and taking the remainder of the table size
  * Hashing is mixing and recombining the value in some way such that it creates as unique a value as possible
  * Universal hashing
    * h(k) = [ ( ak + b ) mod p ] mod m
    * m is table size
    * a and b are random integers [0, p)
    * p is prime number > universe of k ==> large prime
* Hash Table
  * **A generalized array whose indexes can be selected based on a hash of the value stored in that index**
  * Direct addressing = Direct access table = using an array’s index as the key
    * Applicable when we can afford to allocate an array with one position for every possible key
  * Hashing allows us to map keys to values
    * Useful when the number of possible keys is large compared to the actual number of keys
  * When table is full, double table size, like array
  * When number of elements shrinks to size/4, shrink table to size/2 ==> avoids repeated O(n) operations when adding/removing around the doubling/halving number of elements
* Hash Functions
  * Used to transform the key into the index
  * Ideally should map each possible key to a unique slot index, but difficult to achieve in practice
  * An efficient hash function should be designed so that it distributes the index values of inserted objects uniformly across the table
  * Must be calculated quickly, minimize collisions, and efficiently resolve collisions
* Collision = when to records are stored in the same location
* Collision Resolution Techniques
  * Chaining = the value at each index is a linked list, which allows collisions to be resolved by having several elements in a single index
  * Open Addressing
    * **in notes below, `m` indicates the table size**
    * If a collision occurs, the value is stored at a different index
    * **Insert - If initial hash index is filled, probe for a new index until empty or DeleteMe index is found, then insert value in table**
    * **Search - If value at hash index is not desired key, probe for a new index until desired key or None is found**
    * **Delete - If value at hash index is not desired key, probe for a new index until desired key is found, then delete the value at the table and replace with a DeleteMe flag, which indicates that it is empty at present**
    * Linear probing = stored at the next open index
      * h(k, i) = (h'(k) + i) % m
      * may result in poor distribution throughout the table (cluster)
    * Quadratic probing = stored in a subsequent index, progressing in a quadratic manner
      * h(k, i) = (h'(k) + c1*i + c2*i^2) % m
      * may result in a better distribution throughout the table as keys get spread out
    * Double hashing = use a primary hash function for initial index, then combine with a second hash function for the probing sequence
      * h(k, i) = (h1(k) + i*h2(k)) % m
      * h2(k) and m must be relatively prime ==> this can be achieved by making m a power of 2 and making h2 always return an odd number
    * Expected number of probes = 1 / (1 - load factor)
* Load factor = ( # elements in hash table ) / ( size of hash table)
  * If average number of elements in an index > load factor, rehash elements with a bigger hash table size
* Bloom Filter
  * Probabilistic data structure which was designed to check whether an element is present in a set with memory and time efficiency
  * Tells us that element is either NOT in set or MAY BE in set
  * Initialize an array of all zeros
  * When an element is added, use k hash functions to hash the element itself
  * Set the array index to 1 for each of the k indexes yielded by the hash functions
  * If an element is queried and the indexes yielded by the hash functions are not all 1, then the element DEFINITELY HAS NOT been added
  * If they ARE all, then it is likely that it has been added but they all may have had collisions

### Rabin-Karp Algorithm
* Implements rolling hash ADT with O(1) operations
  * `r` maintains an internal string `x`
    * Treat `x` as multi digit number `u` in base a (a = size of alphabet — ASCII)
      * This can be done because the string `x` is an array of its characters, which can be represented as an array of ASCII integers
      * For example, for `a = 10`, `x = [ 1, 2, 3, 4, 5 ] = 1*10^4 + 2*10^3 + 3*10^2 + 4*10^1 + 5*10^0`
      * This is the number that will then be used as input to the hash function
    * **This is advantageous because when rolling, only two O(1) operations must be performed: append new element to end & remove first element**
      * To achieve this:
        * Add element to end: multiply `x[ i+1 : ]` by `a` and add the new element
        * Remove element from front: `x_i - a ^ (L-1) * x[i]`
        * Generalized: `h(x_i+1) = a * ( h(x_i) - a ^ ( L-1 ) * x[i] ) + x[ i + L ] mod m`, where L is the length of s
  * `r.append(char)` = add char to end of `x`
  * `r.append(char)` = delete first char from `x` (assuming it is `char`)
  * `r()` = hash value of x = h(x)
```
# looking for substring s in corpus t

# initialize rolling hash for s = rs
for c in s:
  rs.append(c)

# initialize rolling hash for t = rt
for c in t[:len(s)]:
  rt.append(t)

# check equality
if rs() == rt():
  # substring of t MAY match s
  # check whether s == t[ i-len(s)+1 : i+1 ]
  if s == t[ i-len(s)+1 : i+1 ]:
    return True

# rs and rt not initially equal
# roll through and look for equality
for i in range(len(s), len(t)):
  rt.skip(t[i-len(s)])
  rt.append(t[i])
  if rs() == rt():
    # substring of t MAY match s
    # check whether s == t[ i-len(s)+1 : i+1 ]
    if s == t[ i-len(s)+1 : i+1 ]:
      return True
    else:
      # happens with probability <= 1/s
      pass
```
