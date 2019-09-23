# Kth Smallest Number In Multiplication Table

<b>Question:  Nearly every one have used the Multiplication Table. But could you find out the k-th smallest number quickly from the multiplication table?  
Given the height m and the length n of a m * n Multiplication Table, and a positive integer k, you need to return the k-th smallest number in this table. </b>

```
Example  
Input: m = 3, n = 3, k = 5  
Output:   
Explanation:   
The Multiplication Table:  
1	2	3  
2	4	6  
3	6	9  

The 5-th smallest number is 3 (1, 2, 2, 3, 3).  
```

Thought Process:
* Let's observe the general pattern of the times table:
  * There are m rows each up to length n.
  * Each row has some root value that it increments by from cell to cell:
    * e.g. row 1 increases by +1 from 1, 2, 3, 4, ... n
    * e.g. row 2 increases by +2 from 2, 4, 6, 8, ..., n*2
  * Knowing this, it becomes similar to finding the kth smallest number in m sorted lists
* Maybe if we can think of each row as a linked list fashion in which we can get the head (the guaranteed smallest element) and add this into a priority heap, with some means of replacing it with its next list element?
* Luckily, the `heapq` implementation in Python allows handling of tuples:
  * <i>[Heap elements can be tuples. This is useful for assigning comparison values (such as task priorities) alongside the main record being tracked](https://docs.python.org/2/library/heapq.html#basic-examples)</i>


```python
class Solution:
  def findKthNumber(self, m: int, n: int, k: int) -> int:
    
    # Notice that each row has a root (a value that each cell increases by horizontally up to n-1 times)
    priority_heap = [(j, j) for j in range(1, m+1)]
    heapq.heapify(priority_heap)
  
    val = None
    for _ in range(k):
      val, root = heapq.heappop(priority_heap)
      next_val = val + root
      if next_val <= root * n:
        heapq.heappush(priority_heap, (next_val, root))
    
    return val
```

T = O(kMlog(M))
S = O(M)
