# Search a 2D Matrix II

<b>Question: Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
 * Integers in each row are sorted in ascending from left to right.  
 * Integers in each column are sorted in ascending from top to bottom.  
 </b>
 
```
Example
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Thought Process:
* One route is a binary search type approach in which we compare the solution with the center element (dividing the matrix into 4 quadrants).
  * If the target is equal to the center, we can return True.
  * If the target is less than the center, we can remove the bottom right quadrant.
  * If the target is greater than the center, we can remove the top left quadrant.
  * We can recurse on the 3 smaller quadrants.
  * But this ends up being O(n<sup>log<sub>3</sub>(n)</sup>)
* A better route which is linear is to start at the top-right corner element `(i,j) = (0, n-1)`. 
  * If the target is equal to this value, we can return True.
  * If the target is larger than this element, we can ignore this row (because all elements in the row are smaller than it).
  * If the target is less than this element, we can ignore this column (because all elements in this column are larger than it).

```python
from typing import List 
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]: return False        
        m, n = len(matrix), len(matrix[0])
        i, j = 0, n-1
        while i < m and j >= 0:
          if target == matrix[i][j]:
            return True
          elif target < matrix[i][j]:
            j -= 1
          elif target > matrix[i][j]:
            i += 1
        return False
```

T = O(m+n)  
S = O(1)  

Topics = {Divide and Conquer}
