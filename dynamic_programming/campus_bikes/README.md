# Campus Bikes II

<b>Question:</b>
<br><br>
<b>
On a campus represented as a 2D grid, there are N workers and M bikes, with N <= M. Each worker and bike is a 2D coordinate on this grid.
</b><br>
<b>
We assign one unique bike to each worker so that the sum of the Manhattan distances between each worker and their assigned bike is minimized.
</b><br>
<b>
The Manhattan distance between two points p1 and p2 is Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|.
</b><br>
<b>
Return the minimum possible sum of Manhattan distances between each worker and their assigned bike.
</b>
<br> 

Thought Process:
* We need to do an exhaustive search. Thus, DFS could work quite nicely here: (repeat this for every bike) assign a bike to a worker and then move on and assign one of the remaining bikes to the next worker and so on.
* However, we might notice that some of the work gets repeated.
  * e.g. workers = [a, b, c, d] bikes = [1, 2, 3, 4]. Possible combinations:
    * a1, b2, <b>c3, d4</b>
    * a1, b2, c4, d3
    * a1, b3, c2, d4
    * ...
    * a2, b1, <b>c3, d4</b>
    * ...
  * We should use memoization!
  * We'll use a bitarray to indicate assigned bikes (need a hashable type for memoization!)
  
```python
from typing import Set

class Solution:
  def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> int:
    
    def _dist(wi: int, bi: int) -> int:
      p1, p2 = workers[wi], bikes[bi]
      return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])  
        
    _dp = {}
    def _dfs(wi: int, visited: int):
      '''
      @param wi the worker index
      @param visited a bit array corresponding to bike assignments
      @returns the minimum total distance sum
      '''
      # Base case      
      if wi >= len(workers): return 0
      
      # Handles memoization
      tuple_key = (wi, visited)
      if tuple_key in _dp: return _dp[tuple_key]
      
      # Main DFS procedure here
      min_sum = float('inf')
      for bi in range(len(bikes)):
        if visited & (0b1 << bi) == 0:
          next_visited = visited | 1 << bi
          min_sum = min(_dist(wi, bi) + _dfs(wi + 1, next_visited), min_sum)
      _dp[tuple_key] = min_sum    
      return min_sum
      
    return _dfs(0, 0)
```

T = O(2<sup>n</sup>)  (improved from O(n!) without memoization)  
S = O(mn) where m is the length of `bikes` and n is the length of `workers`  

Topics = {Dynamic Programming, BackTracking}
