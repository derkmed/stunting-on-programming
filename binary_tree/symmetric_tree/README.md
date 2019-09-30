# Symmetric Tree
<b>Question: Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).</b>
```
Example:
This binary tree [1,2,2,3,4,4,3] is symmetric:
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

Thought Process:
* Let's divide the tree in a left half and right half.
* All we want to know is if the left half is the mirror of the right half.
  * Naturally, we can recurse this down, because once we find corresponding subtrees in both halves, we just need to make sure they mirror one another.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
      if not root: return True
      def _isMirror(left_root: TreeNode, right_root: TreeNode) -> bool:
        if not left_root and not right_root: return True
        if not left_root or not right_root: return False

        return (left_root.val == right_root.val) and\
          _isMirror(left_root.left, right_root.right) and\
          _isMirror(left_root.right, right_root.left)
      return _isMirror(root.left, root.right)
```

T = O(n)  
S = O(n)  

Topics = {Tree, Breadth-first Search}
