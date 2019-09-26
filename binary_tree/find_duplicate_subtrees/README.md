# Find Duplicate Subtrees

<b>Question: Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any one of them.
Two trees are duplicate if they have the same structure with same node values.</b>

```
Example
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
      
has subtrees:
      2
     /
    4
    
and

4
```

Thought Process:
* Perhaps for each subtree, we can build a serialized representation of it.
* If we have this serialized data, having a `Counter`/`HashMap` could easily check in constant time whether or not a subtree is duplicated.
* How do we build this serialized representation? 
  * We need an indicator for null nodes.
  * We can apply Depth-first Search to reach the bottom and then proceed to build serialized representations of subtrees from bottom-up.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findDuplicateSubtrees(self, root: TreeNode) -> List[TreeNode]:
        count = collections.Counter()
        ans = []
        
        def collect(node: TreeNode) -> str:
          nonlocal count, ans
          if not node: return "#"
          
          serialized = str(node.val) + "," + collect(node.left) + "," + collect(node.right)
          count[serialized] += 1
          if count[serialized] == 2:
            ans.append(node)
          return serialized
        
        collect(root)
        return ans
```

T = O(N<sup>2</sup>)  (An extra factor of N is considered for string-building)  
S = O(N) (for the serialized data Counter)  

Topics = {Tree, Depth-first Search}
