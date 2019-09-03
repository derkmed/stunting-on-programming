# Spiral Matrix
<b>
Question: Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.


Example 1:  
Input:  
[  
 [ 1, 2, 3 ],   
 [ 4, 5, 6 ],   
 [ 7, 8, 9 ]    
]  
Output: [1,2,3,6,9,8,7,4,5]  
</b>


Thought Process:
* What types of matrices to handle?
  * empty?
  * single-column?
  * single-row?
* Observe how we pass through each side and the bounds will need to change
  * Top edge
  * Right edge
  * Bottom edge
  * Left edge
* There are multiple ways to do this, but hopefully you can the nature of these layers and how we can loop through them.
* Let's keep <i>inclusive</i> index pointers (left, right, bottom, top).
  * We'll allow the top and bottom edges to include the corners
  * We'll disallow the left and right edges from including corners
  * This isn't necessary, but the code becomes a tad bit more concise
  * We just need to make sure that we remember the pointers are inclusive and we break early in the singular row/column cases
```
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
      
      result = []
      if (matrix is None or len(matrix) == 0):
        return result
      bottom, top = len(matrix)-1, 0
      left, right  = 0, len(matrix[0])-1
      while (left <= right and top <= bottom):
        for i in range(left, right+1):
          result.append(matrix[top][i])      
        if top == bottom: break # handle the single-row case
        for j in range(top+1, bottom):
          result.append(matrix[j][right])    
        for i in reversed(range(left, right+1)):
          result.append(matrix[bottom][i])   
        if left == right: break # handle the single-column case
        for j in reversed(range(top+1, bottom)):
          result.append(matrix[j][left])         
        left, right, top, bottom = left+1, right-1, top+1, bottom-1

      return result
```
T = O(n)
S = O(n)
