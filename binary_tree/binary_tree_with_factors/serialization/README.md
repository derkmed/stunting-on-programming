# Serialize and Deserialize Binary Tree

<b>Question: Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.</b>  
  
<b>Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.</b>

```
Example:  
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

Thought Process:
* Let's use pre-order traversal.
* Empty/Null nodes can be indicated by a particular value (e.g. `#` or `X`).
* Serialization is the easy part:
  * We'll put the current root value as a string, followed by a specified delimiter (e.g. `,`.)
  * We can defer the rest of the serialization to recursive calls.
* Deserialization is a bit trickier. Given our pre-order traversal, it's really easy to indicate what should be the root value: it is the first value up to the delimiter! What about the rest?
  * Knowing where to draw the line between deserializing the left subtree and the right subtree is much harder.
  * After handling the root node value, We can parse the serialized data greedily (worry about the current node and then recurse on its c hildren) as long as we stop at the right place: i.e. if we do not append the right subtree incorrectly to the left subtree when deserializing.
  * Luckily, because we have an indicator of when a subtree stops (has no children, has both empty/null nodes), we can get around this issue by having the recursed function call on a child stop immediately (don't keep recursing down that subtree!).
  
  ```python
  # Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from typing import Iterable
class Codec:

  EMPTY = '#'
  DELIMITER = ','
  
  def serialize(self, root: TreeNode) -> str:
    """Encodes a tree to a single string.

    :type root: TreeNode
    :rtype: str
    """
    if not root: return self.EMPTY + self.DELIMITER
    return str(root.val) + self.DELIMITER +\
        self.serialize(root.left) + self.serialize(root.right)

  def deserialize(self, data):
    """Decodes your encoded data to tree.

    :type data: str
    :rtype: TreeNode
    """

    def _deserialize(values: Iterable[str]) -> TreeNode:
      curr_val = next(values)
      if curr_val == self.EMPTY:
        return None
      curr_node = TreeNode(int(curr_val))
      curr_node.left = _deserialize(values)
      curr_node.right = _deserialize(values)
      return curr_node
    if not data: return None
    values = iter(data.split(self.DELIMITER))
    return _deserialize(values)

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
  ```
  
  T = O(N)  
  S = O(N)  
  
  Topics = {Tree, Design}
