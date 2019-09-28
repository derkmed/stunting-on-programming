# Binary Tree Inorder Traversal

<b>Question: Given a binary tree, return the inorder traversal of its nodes' values.</b>

Thought Process:
* There's the really easy recursive solution that comes to mind:
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root: return
        result = []
        left_inorder = self.inorderTraversal(root.left)
        right_inorder = self.inorderTraversal(root.right)
        if left_inorder: result += left_inorder
        result += [root.val]
        if right_inorder: result += right_inorder
        return result
```
* The more interesting solution avoids using any space whatsoever and is known as Morris Traversal.


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:

      curr, parent = root, None
      result = []
      while curr:
        
        # Take value and move right
        if not curr.left:
          result += [curr.val]
          curr = curr.right
          
        # Place parent in rightmost spot of left subtree
        else:
          nxt = curr.left
          while nxt.right: nxt = nxt.right
          nxt.right = curr
          curr = curr.left
          nxt.right.left = None

      return result
```

T = O(N) (you essentially will iterate through each node up to two times)  
S = O(1)  

Topics = {Tree}  
