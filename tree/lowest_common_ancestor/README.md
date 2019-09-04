# Lowest Common Ancestor

<b>Question: Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

Example:
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1  
Output: 3  
Explanation: The LCA of nodes 5 and 1 is 3. 

</b>

Thought Process:
* Perhaps there is a way to traverse down a path and return ssome sort of indication that a path contains either p or q?
  * Recursive element to the solution
* How about we have a recursive helper that will return a node that helps find the LCA or `None`?
* Cases:
  * Is a node `None`?
  * Is a node p?
  * Is a node q?
  * Is a node the LCA?
    * After this is found, how do we propagate this LCA node up the recursive call stack back to be returned at the initial call? 
  * Is the node not p/q/LCA, but helpful in backtracking up to the LCA? How do we return the helpful information  

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
  def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

    def lowestCommonAncestorHelper(root:'TreeNode', p: 'TreeNode', q: 'TreeNode') -> bool: 
      # Base Cases
      if root is None:
        return None
      elif root == p or root == q:
        return root

      # Left + Right Subtrees
      left_candidate = lowestCommonAncestorHelper(root.left, p, q)
      right_candidate = lowestCommonAncestorHelper(root.right, p, q)
      if not left_candidate and not right_candidate:
        return None 
      elif left_candidate and right_candidate:
        return root

      return left_candidate or right_candidate

    return lowestCommonAncestorHelper(root, p, q)
```

T = O(n)   
S = O(n) for the stack frames
