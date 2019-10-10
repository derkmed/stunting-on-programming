# Boundary Traversal

<b>Question: Write a function to print Boundary Traversal of a binary tree. Boundary Traversal of a binary tree here means that you have to print boundary nodes of the binary tree Anti-Clockwise starting from the root.</b>
<b>Note: Boundary node means nodes present at boundary of leftSubtree and nodes present at rightSubtree also including leaf nodes.</b>

Thought Process:
* We can break the problem up into 3 parts:
  1. Print the left boundary in a top-down manner. Exclude the leaf.
  2. Print all leaf nodes from left-to-right.
  3. Print the right boundary in bottom-up manner.
  
```python
class Node:
    def __init__(self, val):
        self.right = None
        self.data = val
        self.left = None

def printBoundaryView(root):
    
    def _printLeftBoundary(root):
        '''
        This will print the left boundaries in a top-down manner, avoiding the leaves.
        '''
        nonlocal result
        if root:
            if root.left:
                result += str(root.data) + " "
                _printLeftBoundary(root.left)
            elif root.right:
                result += str(root.data) + " "
                _printLeftBoundary(root.right)
    
    def _printRightBoundary(root):
        '''
        This will print the right boundaries in a bottom-up manner, avoiding the leaves.
        '''
        nonlocal result
        if root:
            if root.right:
                _printRightBoundary(root.right)
                result += str(root.data) + " "
            elif root.left:
                _printRightBoundary(root.left)
                result += str(root.data) + " "
        
    def _printLeaves(root):
        nonlocal result
        if not root: return 
    
        _printLeaves(root.left)
        if not root.left and not root.right:
            result += str(root.data) + " "
        _printLeaves(root.right)
       
    result = ""
    if root: result += str(root.data) + " "
    _printLeftBoundary(root.left) 
    _printLeaves(root)
    _printRightBoundary(root.right)
    print(result)
```

T = O(n)  
S = O(n) (for the stack frames)

Topics = {Tree}
