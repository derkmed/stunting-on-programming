# Diameter of Binary Tree

<b>Question: Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.</b>


Thought Process:
* A path of n nodes will contain n-1 edges!
* We can count the path that has the most nodes recursively and return the # nodes - 1 (lower bounded by `0`).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
      result = 0
      def _longestPathDistance(root: TreeNode) -> int:
        nonlocal result
        if not root: return 0        
        left = _longestPathDistance(root.left)
        right = _longestPathDistance(root.right)
        result = max(left + right + 1, result)
        return max(left, right) + 1
      
      _longestPathDistance(root)
      return max(result - 1, 0)
```

T = O(n)  
S = O(n) for the recursive call stack frames

Topics = {Tree}
