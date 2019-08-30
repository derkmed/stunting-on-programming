# Binary Tree With Factors

<b>Question: Given an array of unique integers, each integer is strictly greater than 1. We make a binary tree using these integers and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of it's children.

How many binary trees can we make?  Return the answer modulo 10 ** 9 + 7.<br>

Example:  
Input: A = [2, 4]  
Output: 3  
Explanation: We can make these trees: [2], [4], [4, 2, 2]  
</b>

```
class Solution:
    def numFactoredBinaryTrees(self, A: List[int]) -> int:
      dp = {}
      for a in sorted(A):
        # for all elements seen, aggregate them into dp[a] if they are a factor
        dp[a] = sum(dp[k] * dp.get(a / k, 0)  for k in dp if a % k == 0) + 1
      return sum(dp.values()) % (10 ** 9 + 7)
```
T = O(n<sup>2</sup>)<br>
S = O(N)
