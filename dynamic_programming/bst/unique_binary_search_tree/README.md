# Unique Binary Search Tree

<b>Question:</b>

<b>Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?</b>

```
Example
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

Thought Process:
* Notice, that when we solve for n, there are up to n different values that can serve as the root of the tree (1, 2, ..., n).
* Because of the nature of BST, this problem repeats itself among subtrees:  there are `i-1` left subtrees for each root and `n-i` possible right subtrees for each of those `i` possible roots among (1, 2, ..., n).
* `F(n) = Sum(F(i-1) * F(n-i)` for i in range(1, n).

```python
class Solution:
  def numTrees(self, n: int) -> int:
    trees_for = {0: 1, 1: 1}    
    def _numTrees(n: int):
      nonlocal trees_for
      result = 0
      for i in range(n):
        result += trees_for[(n-1)-i] * trees_for[i]      
      trees_for[n] = result
      return result
      
    for i in range(2, n+1):
      _numTrees(i)
    return trees_for[n]
```

Or more concisely:

```python
    trees_for = [0 for _ in range(n + 1)]
    trees_for[0], trees_for[1] = 1, 1
    for i in range(2, n+1):
      for j in range(1, i+1):
        trees_for[i] += trees_for[i-j] * trees_for[j-1]   
    return trees_for[n]
```

T = O(n<sup>2</sup>)  
S = O(n)  

Topics = {Dynamic Programming, Tree}

