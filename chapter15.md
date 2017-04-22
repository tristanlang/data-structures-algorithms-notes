## Chapter 15: String Algorithms

### String Matching Algorithms
* Is pattern P (length m) a substring of string T (length n)?
* Brute Force = O(nm)
```
def brute_force(T, P):
  n = len(T)
  m = len(P)

  for i in range(n-m+1):
    j = 0
    while j < m and T[i + j] == P[j]:
      j += 1

    if j == m:
      return i
```
* Rabin-Karp = O(nm) worst case (bad hash function) but O(n+m) with good hash function
  * See chapter 14 notes
* Knuth-Morris-Pratt (KMP) = O(n+m)
  * Avoids having to backtrack in the text T
  * Uses a prefix table to identify prefixes that are substrings in P of the same characters
  * When a partial match is found in a substring of T but the next character does not match the next desired character in P, check whether the suffix of the matched characters in P is also a prefix in P
  * This is a check as to whether a new successful attempt has already begun, for example, the substrings will match up until the positions indicated
  * The suffix of the successful substring of P `41` is a suffix and the prefix, `3` is the next character that should be checked for equality, since the suffix `41` is known to be correct
```
         |
P = 413412
T = 123413413412
            |

```
  * To achieve this in code, a prefix table must be built based on pattern P, which traverses P looking for internally matching substrings
```
int F[]; // assume F is a global array
public void PrefixTable(int P[], int m) {
  int i = 1, j = 0, F[0] = 0;
  while (i < m) {
    if (P[i] == P[j]) {
      F[i] = j+1
      i++;
      j++;
    } else if (j > 0) {
      j = F[j-1];
    } else {
      F[i] = 0;
      i++;
    }
  }
}
```
  * The prefix table helps the search because it tells you where in the prefix table to look if there is a failed match when looking for P in T
  * The matching component is as follows:
```
public int KMP(char T[],  int n, int P[], int m) {
  int i = 0, j = 0;
  PrefixTable(P, m);
  while (i < n) {
    if (T[i] == P[j]) {
      if (j == m-1) {
        return i-j;
      } else {
        i++;
        j++;
      }
    } else if (j > 0) {
      // have we completed a prefix of P already?
      j = F[j-1];
    } else {
      i++;
    }
  }
  return -1;
}
```

### Data Structures for Storing Sets of Strings
* Hash Tables
  * Good, but loses the location of strings
  * For example, to find all letters beginning with a certain character, the entire table must be scanned
* Binary Search Trees
  * Good in terms of storage, but bad in terms of search time
  * This is because the entire string must be searched when comparing a string vs a target string
* Tries
  * Prefix-based tree
  * Tree in which each node contains pointers that represent up to every letter in the alphabet
  * Insert and Search in O(L) where L is length of a single word
  * When deleting a word, if it has children, just set `endOfWord = false` so as to not delete part of the tree (see https://www.youtube.com/watch?v=AXjmTQ8LEoI)
  * If two strings share a prefix, they will have a common ancestor in the tree
```
TrieNode {
  children map[character]TrieNode
  endOfWord bool
}
```
* Ternary Search Trees (TSTs)
  * Combines aspects of BSTs and tries
  * left points to the TST containing all the strings which are alphabetically less than data
  * right points to the TST containing all the strings which are alphabetically greater than data
  * eq points to the TST containing all the strings which contain `data` as a prefix
```
TSTNode {
  char data;
  boolean endOfWord;
  TSTNode left;
  TSTNode eq;
  TSTNode right;
}
```

### Suffix Trees
* Tree representation of a single string and possible suffixes
* Construction of the tree is complicated but it solves many string problems in O(n) time
  * Exact string matching
  * Longest repeated substring
  * Longest palindrome
  * Longest common substring of two strings
  * Longest common prefix of two strings
* Suffix tree definition for string T of length n
  * contains n leaves which are numbered 1 to n
  * each internal node (except root) should have at least 2 children
  * each edge is labeled by a non-empty substring of T
  * no two edges of a node begin with the same character
  * the paths from root to leaves represent all suffixes of T
* Construction of suffix trees — there exist O(n) algorithms for constructing
