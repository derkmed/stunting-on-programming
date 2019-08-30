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
 ```
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
