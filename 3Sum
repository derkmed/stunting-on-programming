# Valid Triangle Number

<b>Question: Given an array consisting of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.</b>

Thought Process:
* There are nC3 combinations. That leads to approx O(n<sup>3</sup>) iterations.
  * If we are smart however, maybe we can bring this down to O(n<sup>2</sup>)?
* A valid triangle consists of 3 sides `a`, `b`, `c` such that `a + b > c`, for all possible assignments of `a`, `b`, `c` to each side. 
* This is very similar to 3Sum. 
  * How about, we sort in descending order. This way, if we move from left-to-right, we know  the maximum size `c` that `a + b` must be greater than
  
```
class Solution:
  def triangleNumber(self, nums: List[int]) -> int:
    nums, count_result, n = sorted(nums, reverse=True), 0, len(nums)
    for i in range(n):
      j, k = i + 1, n - 1
      while j < k:
        if nums[j] + nums[k] > nums[i]:
          count_result += k - j
          j += 1
        else: 
          k -= 1
    return count_result
```
