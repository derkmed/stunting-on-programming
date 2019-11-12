# Maximum Subarray

<b>Question</b>

<b>Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.</b>

```
Example:
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
Thought Process:
* Iterate linearly through each element while keeping track of the maximum sum seen so far as well as the maximum sum ending at this index.

```python
class Solution:
  def maxSubArray(self, nums: List[int]) -> int:
    max_so_far, max_ending_here = nums[0], nums[0]
    for num in nums[1:]:
      max_ending_here = max(num, max_ending_here + num)
      max_so_far = max(max_ending_here, max_so_far)
    return max_so_far

```

T = O(n)  
S = O(1)  

Topics = {Dynamic Programming}
