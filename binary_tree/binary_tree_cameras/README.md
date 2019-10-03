# Binary Tree Cameras

<b>Question: Given a binary tree, we install cameras on the nodes of the tree. Each camera at a node can monitor its parent, itself, and its immediate children. Calculate the minimum number of cameras needed to monitor all nodes of the tree.</b>

```
Example:
Input: [0,0,null,0,0]
Output: 1
Explanation: One camera is enough to monitor all nodes if placed as shown.
```

```
Example:
Input: [0,0,null,0,null,0,null,null,0]
Output: 2
Explanation: At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.
```

Thought Process:
* Let's think about this case at the node level, as we try to do for all binary tree type problems.
  * A camera is placed here.
    * case A.) The root and subtree below should be covered
  * A camera is not placed here.
    * case B.) The root and subtree below are already covered.
    * case C.) The subtree below should be covered: the root will have to be covered by its parent.
* We can recurse on these three cases and keep camera counts for each different scenario. 
* Remember:
  * The tree camera placement does not necessarily have to be symmetric.
  * The root will have to be covered:  if we return the minimum counts, we can not return the case in which the root relies on a parent for coverage.
* We can derive a formula for each case, if we think carefully.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from typing import Tuple

class Solution:
  def minCameraCover(self, root: TreeNode) -> int:
        
    def _minCameraCover(root: TreeNode) -> Tuple[int, int, int]:
      '''
      cases:
      1. subtree covered + node not covered + no cam here => parent must have cam
      2. subtree coverd + node covered + no cam here => parent does/not have cam
      3. subtree + node covered + cam here => parent does/not have cam
      '''     
      if not root: return 0, 0, float('inf')
      l1, l2, l3 = _minCameraCover(root.left)
      r1, r2, r3 = _minCameraCover(root.right)
      
      n1 = l2 + r2 # neither children can have a cam
      n2 = min(l3 + min(r2, r3), r3 + min(l2, l3)) # one of the children needs a cam
      n3 = min(l1, l2, l3) + 1 + min(r1, r2, r3)
      return n1, n2, n3
    return min(_minCameraCover(root)[1:])
```

T = O(N)  
S = O(N)  

Topics = {Tree, Dynamic Programming}
