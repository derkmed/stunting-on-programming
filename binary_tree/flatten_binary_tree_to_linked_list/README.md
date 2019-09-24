# Flatten Binary Tree to Linked List

<b>Question: Given a binary tree, flatten it to a linked list in-place. For example, given the following tree:</b>

```
Example:
    1
   / \
  2   5
 / \   \
3   4   6
```

```
Example:
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```
Thought Process:
* This is just in-order traversal. What's tricky however is that we don't actually return a traditional Linked List.
* We return a tree-representation of one by using te `.right` attribute.
* We can recurse as long as we handle the empty node case.
* We then flatten the left subtree. Because `flatten()` should return a root, it should return the root of the flattened left subtree if we call it on that. 
  * Set `root.right` to this flattened subtree, after saving a reference to the right subtree.
* We should now have our "Linked List" and it should end in null. Iterate through this list to find the last non-null element and set its `.right` to the flattened right subtree.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
  def flatten(self, root: TreeNode) -> None:
    if not root: return None
    right_reference = root.right
    root.right = self.flatten(root.left)
    root.left = None
    
    tmp = root
    while tmp.right: tmp = tmp.right
    tmp.right = self.flatten(right_reference)
    return root
```

T = O(n)  
S = O(n)  

Topics = {Tree}
