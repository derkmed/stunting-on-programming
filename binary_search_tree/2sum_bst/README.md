# Two Sum - Input is a BST

<b>Question: Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.</b>

```
Example:
Input: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```

Thought Process:
* Let's keep a set of values we need (`k - root.val`) as we iterate down the tree. We can also use a queue to keep track of the subtrees we've yet to expand.
* If a node that gets visited shares this needed value, we can return True.

```python
from collections import deque
class Solution:
    def findTarget(self, root: TreeNode, k: int) -> bool:
        l, r = root, root
        if root.val + root.val == l: return True
        needed = set()
        queue = deque([root])
        while queue:
            node = queue.popleft()
            if not node: continue
            if node.val in needed: return True
            needed.add(k - node.val)
            queue.append(node.left)
            queue.append(node.right)
        queue = deque([root.right])
```

T = O(N)  
S = O(N)  

Topics = {Tree}
