# Trapping Rain Water 1

Thought Process:
* Each index is bounded by `min(max_on_right, max_on_left)`
* We do a shrinking window pointer comparison with `left` and `right` pointers while`left <= right`
  * Let `left_max` be the current maximum height from left side (that is from `heights[0, 1, ... left]`)
  * Let `right_max` be the maximum height from right side(from `heights[right, right+1, ... n-1]`).
  * Let `height[h]` be the maximum height such that `left < h < right`. 
  * Let's define `"<<"/">>"` as "relatively larger" than their `"<"/">Th` counterparts. i.e. 4 >> 1 when comparing with 2 > 1 because `4=1+3` whereas `2=1+1` and `3 > 1`. 
  * Six main cases arise:
   1. `left_max < height[h] < right_max` (e.g. `[1, ... 3, ... 5]`): water @ `left` is bounded by `left_max`
   2. `left_max << height[h] > right_max` (e.g. `[1, ... 5, ... 3]`): water @ `left` is bounded by `left_max`
   3. `left_max > height[h] << right_max` (e.g. `[3, ... 1, ... 5]`): water @ `left` is bounded by `left_max` 
   4. `left_max < height[h] >> right_max` (e.g. `[3, ... 5, ... 1]`): water @ `right` is bounded by `right_max`  
   5. `left_max >> height[h] < right_max` (e.g. `[5, ... 1, ... 3]`): water @ `right` is bounded by `right_max` 
   6. `left_max > height[h] > right_max` (e.g. `[5, ... 3, ... 1]`): water @ `right` is bounded by `right_max`
  * Ignore the case in which `left_max == right_max`. These become trivial because the upper bound of water trapped @ either `left` or `right` is the same. 
  * Notice that 6 cases essentially simplify down to a simple comparison between `left_max` and `right_max`.
  1. The first 3 modify `left` because `left_max < right_max`
  2. The latter 3 modify `right` because `right_max > left_max`
  * Using this comparison, we can make left and right one step closer. Thus we can finish it in linear time.
 ```python
class Solution:
    def trap(self, heights: List[int]) -> int: 
      n = len(heights)
      left_max, right_max = 0, 0
      result = 0
      
      l, r = 0, n-1
      while (l <= r):
        left_max = max(left_max, heights[l])
        right_max = max(right_max, heights[r])
        if left_max < right_max:
          result += left_max - heights[l]
          l += 1
        else:
          result += right_max - heights[r]
          r -= 1
      return result 
 ```  
 T = O(n)   
 S = O(1)   
 
# Trapping Rain Water 2

<b>Question: Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.  

Given the following 3x6 height map:  
\[  
  \[1,4,3,1,3,2\],  
  \[3,2,1,3,2,4\],  
  \[2,3,3,2,3,1\]  
\]  
  
Return 4.
</b>  

Thought Process:
* Similar to <b>Trapping Rain water 1</b>, but now we have more than two pointers. The boundary of the container starts at the four edges of the matrix (rectangular boundary)
* How do we find the minimum of the boundary? This is no longer as straightforward as in the 1D case due to the rectangular boundary.
  * Fortunately we have a data structure called "PriorityQueue" which can help find the minimum in log(queue_size) time.
* How do we replace the boundaries and how do we avoid repetition? Each cell can have at most four neighbors.
  * Let's keep a `visited` data structure.

```python
import heapq
from typing import Tuple

class Solution:
  def trapRainWater(self, height_map: List[List[int]]) -> int:

    if height_map is None or len(height_map) < 1: return 0

    priority_queue = []
    m, n = len(height_map), len(height_map[0])
    visited = [[False] * n for _ in range(m)]

    for i in range(m):
      for j in range(n):
        if i == 0 or i == m-1 or j == 0 or j == n-1:
          heapq.heappush(priority_queue, (height_map[i][j], i, j))
          visited[i][j] = True

    result = 0
    while priority_queue:
      h, i, j = heapq.heappop(priority_queue)
      for ni, nj in [(i+1, j), (i-1, j), (i, j+1), (i, j-1)]:
        if ni >= 0 and ni < m and nj >= 0 and nj < n and not visited[ni][nj]:
          result += max(0, h - height_map[ni][nj])
          heapq.heappush(priority_queue, (max(h, height_map[ni][nj]), ni, nj))
          visited[ni][nj] = True

    return result
```

T = O(mnlogmn)  where (m, n) are (row, columns). The log(mn) is for pushing elements onto the priority queue.  
S = O(mn)    
