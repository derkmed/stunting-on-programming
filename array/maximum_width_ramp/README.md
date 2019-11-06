# Maximum Width Ramp

<b>Question:</b>
<b>Given an array A of integers, a ramp is a tuple (i, j) for which i < j and A[i] <= A[j].  The width of such a ramp is j - i. Find the maximum width of a ramp in A.  If one doesn't exist, return 0. </b>


Thought Process:


```python
class Solution:
  def maxWidthRamp(self, A: List[int]) -> int:
    
    # We want to sort the indices based on the value-@-them constraint
    B = sorted([i for i in range(len(A))], key=A.__getitem__)
    
    result = 0
    min_seen = float('inf')
    for i in B:
      result = max(i - min_seen, result)
      min_seen = min(i, min_seen)
      
    return result
```

T = O(nlogn)  
S = O(n)  

Topics = {Array}
