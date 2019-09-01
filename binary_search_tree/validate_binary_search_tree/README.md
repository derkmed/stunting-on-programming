# Validate Binary Search Tree

<b>Question: 
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
</b>

Thought Process:
* If a root node is a valid BST, both its children must also be valid subtree-BST
  * There is a recursive nature
* Somehow, we need to make sure that:
  * <b>All</b> elements on the left of a root node have values smaller than the root node value
  * <b>All</b> elements on the right of a root node have values larger than the root node value
* In order to guarantee this, we'll need to somehow pass the value-bounding information as we recurse

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
      
      def isValidBSTHelper(root: TreeNode, min_value_above: int = float('-inf'), max_value_above: int = float('inf')) -> bool:
        if not root:
          return True

        val = root.val
        if val <= min_value_above or val >= max_value_above:
          return False
        return isValidBSTHelper(root.left, min_value_above, val) and isValidBSTHelper(root.right, val, max_value_above)
     
      return isValidBSTHelper(root)
``` 

T = O(n)  
S = O(n)
