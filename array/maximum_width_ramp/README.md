# Maximum Width Ramp

<b>Question:</b>
<b>Given an array A of integers, a ramp is a tuple (i, j) for which i < j and A[i] <= A[j].  The width of such a ramp is j - i.</b>
<b>Find the maximum width of a ramp in A.  If one doesn't exist, return 0. </b>


Thought Process:
* This is very similar to the "max stock profit problem."
* We essentially want to maximize the distance between (i, j) such that A[i] <= A[j].
  * We can run the max stock profit problem solution (linear iteration keeping track of the minimum seen so far and the largest gap seen so far) on the indices.
  * However, we need to constrain the order of indices based on the values at those indices. Thus, we sort the range of indices based on their corresponding array values.
  

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
