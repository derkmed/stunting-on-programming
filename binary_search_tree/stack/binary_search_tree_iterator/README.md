# Binary Search Tree Iterator

<b>Question:</b>

<b>Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST. Calling next() will return the next smallest number in the BST.</b>

<b>Notes:</b>
* <b>next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.</b>
* <b>You may assume that next() call will always be valid, that is, there will be at least a next smallest number in the BST when next() is called.</b>

Thought Process:
* Consider the structure of a BST: given a root, the nodes on the left are smaller and the nodes on the right are larger.
* Keep a growing stack of nodes to visit, with the invariant that the next smallest node is on the top.
  * This stack will essentially come from iterating among all left nodes from the root down.
  * Whenever you pop a node, you must expand its right (the left will have already been visited, naturally).
  
  
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator:

    def __init__(self, root: TreeNode):
      self.root = root
      self.stack = []
      tmp = self.root
      self.expandLeft(self.root)

    def expandLeft(self, node: TreeNode):
      while node:
        self.stack.append(node) 
        node = node.left
        
    def next(self) -> int:
        """
        @return the next smallest number
        """
        if not self.stack: return None
        next_node = self.stack.pop()
        if next_node.right:
          self.expandLeft(next_node.right)
        return next_node.val
          
    def hasNext(self) -> bool:
        """
        @return whether we have a next smallest number
        """
        return len(self.stack) > 0


# Your BSTIterator object will be instantiated and called as such:
# obj = BSTIterator(root)
# param_1 = obj.next()
# param_2 = obj.hasNext()
```

T = O(1)  
S = O(h)  

Topics = {Binary Search Tree, Stack}
