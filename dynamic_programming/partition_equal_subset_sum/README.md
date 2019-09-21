# Partition Equal Subset Sum

<b>Question: Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.</b>

<b>Note:</b>
* <b>Each of the array element will not exceed 100.</b>
* <b>The array size will not exceed 200.</b>

```
Example:  
Input: [1, 5, 11, 5]  
Output: true  
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

Thought Process:
* This is similar to the iconic dynamic programming subset sum problem. The difference is that here we want to see if there exists a subset in which the sum is exactly half of the sum of the array.
  * We know that any provided array that has an uneven sum can return `False`: it's impossible to equally divide this.
* Instead of using a 2-dimensional memoization table, we can reduce space complexity down to a single dimension, as long as we are smart with the order in which we process values.

```python
class Solution(object):
    def canPartition(self, nums: List[int]) -> bool:
        total_sum = sum(nums)
        if total_sum % 2 != 0: return False
        dp = [False] * (total_sum / 2 + 1)
        dp[0] = True
        for i in range(len(nums)):
            for j in range(len(dp)-1, 0, -1):
                dp[j] = dp[j] | (dp[j - nums[i]] if j - nums[i] >= 0 else False)
        return dp[-1]    
```
T = O(N)  
S = O(N)  

Topics: {Dynamic Programming}
