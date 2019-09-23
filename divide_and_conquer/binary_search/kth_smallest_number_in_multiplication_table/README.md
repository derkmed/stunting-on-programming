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
* A suboptimal solution exists that runs in O(kMlog(M)) using a Priority Queue. Let's do better.
* Notice that it is possible to actually do binary search in this problem and produce a more optimal O(Mlog(MN)) runtime complexity for when k gets really large.
* If we think about binary search, we depend on some indicator to determine how to reduce our currenty [lo:hi] window of all elements (to either the lower half `[:mid] or higher half[mid+1:]).
  * What if we use a defined function `hasKSmallerElements(x)` which will return True when we know that there most definitely are k values smaller than x in the provided multiplication table (given the provided confines)?
  * We know that each row exhibits the pattern `[i, 2*i, 3*i, ..., n*i]`. To determine if there are indeed k elements in this row smaller than a provided x, well that's simply just `min(n, x // i)`.
    * This comes from knowing that in each row (not considering the n bound), there can potentially exist some product `k*i : k*i <= x`. This would for sure indicate there are k values in the ith row smaller than or equal to x.
    * If this product `k*i` is out of bounds (`k * i > n`) however, then we may need to check more of the other m-1 rows (`!= i`) to verify that there are indeed k smaller elements than x.
  * If for a particular element x, there are indeed k smaller elements than x, than we can reduce our window to look at [:mid]. Else, reduce our window to [mid+1:].

```python
class Solution:
  def findKthNumber(self, m: int, n: int, k: int) -> int:
    
    def hasKSmallerElements(x):
      count = 0
      for i in range(1, m+1):
        count += min(n, x // i)
      return count >= k
      
    lo, hi = 1, m*n
    while lo < hi:
      mid = (lo + hi) // 2
      if hasKSmallerElements(mid):
        hi = mid
      else:
        lo = mid + 1
    return lo
```

T = O(mlog(mn))  
S = O(1)    
  
Topics = {Binary Search}  
