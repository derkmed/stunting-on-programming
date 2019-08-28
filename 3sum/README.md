# 3Sum
<b>Question: Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero. The solution set must not contain duplicate triplets.</b>

Thought Process:
* There are nC3 combinations. That leads to approx O(n<sup>3</sup>) iterations.
  * If we are smart however, maybe we can bring this down to O(n<sup>2</sup>)?
* What if we first sort the list?
  * We can iterate through each element `i` in `[0, n - 2)` and for each element, we can have a shrinking window from `[i + 1, n - 1]`
* We need to consider at what point can we immediately break out? 
  * When the smallest number is larger than 0, there's no point continuing
* We need to consider possible edge cases
  * There may be duplicate elements, and we do not want to return duplicate triplets.

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
      triplets = []
      nums, n = sorted(nums), len(nums)
      for i in range(n - 2):
        if (nums[i] > 0): break
        if (i > 0 and nums[i] == nums[i - 1]): continue
          
        l, r = i + 1, n - 1
        while l < r:
          curr_sum = nums[i] + nums[l] + nums[r]
          if (curr_sum < 0):
            l += 1
          elif (curr_sum > 0):
            r -= 1
          else:
            triplets.append([nums[i], nums[l], nums[r]])            
            while l < r and nums[l] == nums[l + 1]:
              l += 1
            while r > l and nums[r] == nums[r - 1]:
              r -= 1
            l += 1
            r -= 1
            
      return triplets       
```

T=O(n<sup>2</sup>)<br>
S=O(n<sup>2</sup>)

# 3Sum Closest

<b>Question: Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.</b>

Thought Process:
* Similar to regular 3Sum, but now we want the value closest to `target` as opposed to `0`
* Start with the initial default solution (`nums[0] + nums[1] + nums[2]`) as a placeholder and then run the regular 3Sum logic from there.

```
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums, n = sorted(nums), len(nums)
        solution = nums[0] + nums[1] + nums[2]
        solution_distance = target - solution
        
        for i in range(0, n - 2):
          l, r = i + 1, n - 1   
          while l < r:
            curr_sum = nums[i] + nums[l] + nums[r]
            candidate_dist = abs(curr_sum - target)
            if (candidate_dist < solution_distance): 
                solution, solution_distance = curr_sum, candidate_dist
            if (curr_sum < target):
              l += 1
            elif (curr_sum > target):
              r -= 1
            else:
              return curr_sum
              
        return solution
```

T=O(n<sup>2</sup>)<br>
S=O(1)


# Valid Triangle Number

<b>Question: Given an array consisting of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.</b>

Thought Process:
* There are nC3 combinations. That leads to approx O(n<sup>3</sup>) iterations.
  * If we are smart however, maybe we can bring this down to O(n<sup>2</sup>)?
* A valid triangle consists of 3 sides `a`, `b`, `c` such that `a + b > c`, for all possible assignments of `a`, `b`, `c` to each side. 
* This is very similar to 3Sum. 
  * How about, we sort in descending order. This way, if we move from left-to-right, we know  the maximum size `c` that `a + b` must be greater than.
  
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
T=O(n<sup>2</sup>)<br>
S=O(1)
