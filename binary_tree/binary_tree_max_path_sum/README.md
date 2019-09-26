# Binary Tree Maximum Path Sum

<b>Question: Given a non-empty binary tree, find the maximum path sum. For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root.</b>

```
Example:
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

Thought Process:
* If we generalize the problem to what work must be done at a specific node, and what work can be deferred to the other nodes:
  * the possible paths can:
  1. go through this node
  2. end at this node or below
* For paths that go through this node:
  * through the left subtree with this node and up
  * through the right subtree with this node and up
  * through this node and up
* For paths that end at this node or below:
  * in the left subtree with/out this node
  * in the right subtree with/out this node
  * in the left and right subtree through this node
  * just this node
* We can recurse down and come back up with the max subtree sums, but we'll need to know the following information with each function call:
  * the max path sum that goes through this node (as we recurse upwards)
  * the max path sum seen overall so far
* We can use a global variable for the latter, or we can just nest both of these values in a tuple.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from typing import Tuple
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
      max_sum = -float('inf')     
      def _maxPathSum(root: TreeNode) -> int:
        nonlocal max_sum
        if not root: return -float('inf')
        
        r_sum = _maxPathSum(root.right) + root.val # through the right child (the non-include this node case caught by the right-child)
        l_sum = _maxPathSum(root.left) + root.val # through the left child (the non-include this node case caught by the left-child)
        pivot_end_sum = r_sum + l_sum - root.val        
        max_sum = max(pivot_end_sum, r_sum, l_sum, root.val,  max_sum)
        
        return max(r_sum, l_sum, root.val) # these are the only three paths that can propagate up
        
      _maxPathSum(root)
      return max_sum
```

T = O(N)
S = O(N)

Topics = {Trees, Depth-first Search }
  
  
