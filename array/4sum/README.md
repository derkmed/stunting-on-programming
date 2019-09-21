# 4Sum

<b>Question: Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:

The solution set must not contain duplicate quadruplets. </b>

Thought Process:
* Very similar to 3Sum. 
* How about we choose a starting element for each element `i ~ [0,len(nums)-3]` and call 3Sum on the remainder of the array?

```python
  class Solution:
  
  def threeSumSorted(self, nums: List[int], start_index: int, target: int) -> List[List[int]]:
    solutions = []
    for i in range(start_index, len(nums) - 2):
      if i > start_index and nums[i] == nums[i-1]:
        continue
        
      target_given_i = target - nums[i]
      j, k = i+1, len(nums) - 1
      while j < k:
        if nums[j] + nums[k] > target_given_i:
          k -= 1
        elif nums[j] + nums[k] < target_given_i:
          j += 1
        else:
          solutions.append([nums[i], nums[j], nums[k]])
          while j < k and nums[j+1] == nums[j]:
            j += 1
          while j < k and nums[k-1] == nums[k]:
            k -= 1
          j += 1
          k -= 1
    return solutions
          
  def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
    nums.sort()
    solutions = []
    for i in range(len(nums) - 3):
      if i > 0 and nums[i] == nums[i-1]:
        continue
      three_sum_solutions = self.threeSumSorted(nums, i+1, target - nums[i])
      for s in three_sum_solutions:
        solutions.append([nums[i]] + s)
    return solutions
```

T = O(n<sup>3</sup>)  
S = O(n<sup>4</sup>) the number of solutions
