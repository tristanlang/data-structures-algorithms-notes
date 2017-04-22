## Chapter 11: Searching
* Unordered linear search = O(n)
* Binary Search = O(log n)

```
def binary_search_recursive(array, val, lo, hi):
  if lo > hi:
    return -1

  mid = lo + (hi - lo)/2
  if array[mid] == val:
    return mid
  elif array[mid] < val:
    return binary_search_recursive(array, val, mid+1, hi)
  else:
    return binary_search_recursive(array, val, lo, mid-1)
```

```
def binary_search_iterative(array, val):
  lo = 0
  hi = len(array) - 1
  
  while lo <= hi:
    mid = lo + (hi - lo)/2
    if array[mid] == val:
      return mid
    elif array[mid] < val:
      lo = mid + 1
    else:
      hi = mid - 1

  return -1
```
