# Minimum Absolute Difference in BST

<b>Question: Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.</b>

```
Example:
Input:

   1
    \
     3
    /
   2

Output:
1

Explanation:
The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).
```

Thought Process:
* This is a BST: there is a proper ordering of elements if we're smart. 
* If we have a sorted collection of elements, we can easily obtain the minimum absolute difference in O(n) time by simply getting the minimum of the differences between consecutive elements.
  * How do we extend this thought to BSTs?
  * What if we can somehow visit each node in increasing order? Consider In-order traversal (left->root->right)?
*Logic:  
  * First handle the base case of root being None/null. 
  * What if we recurse all the way down to the leftmost child (the smallest element in the subtree).
    * We then save a reference to this (last seen node)
  * When we return from the recursion call, we can compare the minimum value seen with the current node value - last seen node value (`root.val-prev_node.val`). If the new difference is smaller, we have a new minimum absolute difference.
    * We then save a reference to this (last seen node)
  * Next we can proceed to the right child. 
    * Because we have a saved reference of the last seen node (the one representing the value that comes right before the current), the logic for obtaining the minimum absolute difference can be repeated.
    
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
  
  def getMinimumDifference(self, root: TreeNode) -> int:
    min_seen = float('inf')
    prev_node = None
    
    def _getMinimumDifference(root: TreeNode) -> None:
      nonlocal min_seen, prev_node
      if not root: return 
      
      _getMinimumDifference(root.left)      
      if prev_node:
        min_seen = min(min_seen, root.val - prev_node.val)        
      prev_node = root
      _getMinimumDifference(root.right)
    
    _getMinimumDifference(root)
    return min_seen
```
 
 T = O(N)  
 S = O(1)

Toics = {Tree}
