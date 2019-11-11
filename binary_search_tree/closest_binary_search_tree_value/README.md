# Closest Binary Search Tree Value

<b>Question:</b>

<b>Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.</b>  

<b>
Note:
Given target value is a floating point.
You are guaranteed to have only one unique value in the BST that is closest to the target.
</b>

```
Example:

Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
```

Thought Process:
* In a BST:
  * the values in the left subtree are < the value at the root
  * the values in the right subtree are > the value at the root
* Let's choose which subtree to explore based off of whether or not `target < root.val` or `target > root.val` (if `target == root.val`, we found the unique solution).


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    
    def closestValue(self, root: TreeNode, target: float) -> int:

      min_dist = float('inf')
      result = root.val if root else None
      def _closestValue(root: TreeNode, target: float) -> int:
        nonlocal min_dist, result
        if root:
          root_dist = abs(root.val - target)
          if min_dist > root_dist:
            min_dist = root_dist 
            result = root.val
          if target < root.val: _closestValue(root.left, target)
          elif target > root.val: _closestValue(root.right, target)

      _closestValue(root, target)
      return result
```

T = O(logn)
S = O(1)  

Topics = {Binary Search Tree}
