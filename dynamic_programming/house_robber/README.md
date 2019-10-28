# House Robber

<b>Question:</b>

<b>
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night. Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.
</b>

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

Thought Process: 
* The typical DP way of solving this would be to work from right to left based off the fact that the `answer at nums[i] = max(nums[i] + answer at nums[i-2], nums[i-1])`.
* We can save space however, but noticing only two variables are needed for any particular index (one holding the answer at nums[i-1] and one holding the answer at nums[i-2]).

```python
class Solution:
  
  def rob(self, nums: List[int]) -> int:
    if not nums or len(nums) == 0: return 0
    
    one_behind = 0
    two_behind = 0
    for num in nums:  
      tmp = one_behind
      one_behind = max(num + two_behind, one_behind)  
      two_behind = tmp
    return one_behind
```

T = O(n)  
S = O(1)   

Topics = {Dynamic Programming}
