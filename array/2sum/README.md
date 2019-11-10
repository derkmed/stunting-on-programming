# Two Sum Less Than K

<b>Question:</b>

<b>Given an array A of integers and integer K, return the maximum S such that there exists i < j with A[i] + A[j] = S and S < K. If no i, j exist satisfying this equation, return -1.</b>

Thought Process:
* This is a simple variant of the traditional 2Sum problem. However, when we encounter a sum that is `== K`, instead of return the sum, we must continue searching.
  * Because of the typical "closing-window" approach, the sum will have to be downsized by the `right` element. However, we cannot stop there because we can continue to increase the size of the `lefty` element. 

```python
class Solution:
  def twoSumLessThanK(self, A: List[int], K: int) -> int:
    A.sort()
    result = -1
    l, r = 0, len(A)-1
    while l < r:
      curr_sum = A[l] + A[r]
      if curr_sum >= K: 
        r -= 1
      else:
        result = max(curr_sum, result)
        l += 1
    return result
```

T = O(nlogn)  
S = O(1)  

Topics = {Array Sort}
