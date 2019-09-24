# Lowest Common Ancestor of a Binary Search Tree

<b>Question: Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST. According to the defintion of LCA on Wikipedia: "The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).</b>

```
Example:
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8  
Output: 6  
Explanation: The LCA of nodes 2 and 8 is 6. 
```

Thought Process:
* One approach we can take is to consider each node abstractly. A node will either be the LCA or contain the LCA in its left or right subtrees. 
* The question, is which subtree is it?
  * We can recurse both ways, and once we know, unravel the recursion to return with some indication that `p` or `q` exists in this subtree.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root: return None        
        if root == p or root == q: return root
        
        leftNode = self.lowestCommonAncestor(root.left, p, q)
        rightNode = self.lowestCommonAncestor(root.right, p, q)
        if leftNode and rightNode: return root
        elif leftNode: return leftNode
        elif rightNode: return rightNode      
        return None
```
T = O(n)  
S = O(n)  (for the stack frames)

Thought Process:
* A better approach is to observe that this is a BST. 
* Given any current node, we know which subtrees `p` and `q` are located in based off how their values compare with the root value.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root: return None        
        
        node = root
        while node:
          if p.val > node.val and q.val > node.val:
            node = node.right
          elif p.val < node.val and q.val < node.val:
            node = node.left
          else:
            return node
      
        return None
```

T = O(n)  
S = O(n) 

Topics = {Tree}
