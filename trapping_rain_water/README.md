# Trapping Rain Water 1





Thought Process:
* Each index is bounded by `min(max_on_right, max_on_left)`
* We do a shrinking window pointer comparison with `left` and `right` pointers while`left <= right`, we observe that the current maximum height from left side (that is from `heights[0,1...left]`) and the maximum height from right side(from `heights[right,right+1...n-1]1). 
* if (`max_on_left < max_on_right`) then, at least (`leftmax - heights[left]`) water can definitely be stored no matter what happens between left, right] since we know there is a barrier at the `rightside(rightmax>leftmax)`.
* On the other side, we cannot store more water than (`max_on_left - heights[left]`) at index a since the barrier at left is of height leftmax. So, we know the water that can be stored at index `left` is exactly (`max_on_left - heights[left`). 
* The same logic applies to the case when (`max_on_left > max_on_right`). At each loop we can make left and right one step closer. Thus we can finish it in linear time.
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
