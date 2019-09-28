# Target Sum

<b>Question: You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.
Find out how many ways to assign symbols to make sum of integers equal to target S.</b>

```
Example:
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

Thought Process:
* This is a variation of the subset sum problem. However, our range of values is now from `[0,k]` to `[-k,k]`. 
  * What if we shift this over to `[0, 2k]` for indexing purposes in an array?
  * Alternative could be to use a HashMap/Dictionary.
* We can keep a memoized table of rows and iterate from top to bottom to implement dynamic programming. For any cell (i,j), it's value dp[i][j] = dp[i-1][j-nums[i]] + dp[i-1][j+nums[i]] (because we can add or subtract an element).
* Ignore out of bounds cells. 
* Further optimize by only storing 2 rows at a time (the previous row and the current row).

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        
        nums_sum = sum(nums)
        ways_length = nums_sum*2+1
        prev_ways = [0] * ways_length
        prev_ways[nums_sum] = 1
        if S < -nums_sum or S > nums_sum: return 0
        
        for num in nums:
          print(prev_ways)
          curr_ways = [0] * ways_length
          for i in range(ways_length):
            added = prev_ways[i+num] if i+num < ways_length else 0
            subbed = prev_ways[i-num] if i-num >= 0 else 0
            curr_ways[i] += added + subbed

          prev_ways = curr_ways 
        return (curr_ways[S+nums_sum])
```

T = O(LN) where L refers to the sum(nums)*2+1  
S = O(L)  

Topics = {Dynamic Programming}
