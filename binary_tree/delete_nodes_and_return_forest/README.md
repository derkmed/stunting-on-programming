# Delete Nodes and Return Forest

<b>Question: Given the root of a binary tree, each node in the tree has a distinct value. After deleting all nodes with a value in to_delete, we are left with a forest (a disjoint union of trees). Return the roots of the trees in the remaining forest.  You may return the result in any order.</b>

```
Example:
Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
Output: [[1,2,null,4],[6],[7]]
```

Thought Process
* There are two important considerations to keep in mind at the particular node:
  1. Is this a potential new "root"? Is it's parent supposed to be deleted?
  2. Is this a node that must be deleted? How does its parent handle it?
* Potential cases:
  * Empty node case
  * We delete the current node.
    * Its children are potential new roots (if they are not to be deleted): can they handle this themselves?
  * We do not delete the current node.
    * It is potentially a new root (if its parent was deleted).
    * Its children references may need to be deleted if either of them are to be deleted.
* What if we have a helper function that returns the root of the current subtree if it should not be deleted? Else, it returns `None`/`null`.
 
  
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from typing import Set
class Solution:
  def delNodes(self, root: TreeNode, to_delete: List[int]) -> List[TreeNode]:
    result = []
    delete_set = set(to_delete)

    def _delNodes(curr: TreeNode, is_root: bool, to_delete: Set[int]) -> TreeNode:
      nonlocal result

      # Base case
      if not curr: return None    

      delete_curr = curr.val in to_delete
      # This node is a root
      if not delete_curr and is_root:
        result += [curr]

      curr.left = _delNodes(curr.left, delete_curr, to_delete)
      curr.right = _delNodes(curr.right, delete_curr, to_delete)          
      return None if delete_curr else curr

    _delNodes(root, True, delete_set)
    return result
```

T = O(n)  
S = O(n) for stack frames

Topics = {Tree, Depth-First Search}
