# Set Matrix Zeroes

<b>
Question: Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.<br>

Example:  
Input:  
[  
  [0,1,2,0],  
  [3,4,5,2],  
  [1,3,1,5]  
]  

Output:    
[  
  [0,0,0,0],  
  [0,4,5,0],  
  [0,3,1,0]  
]  
</b>  

Thought Process:
* We need some way to indicate whether or not a row/column must be set to all zeroes. 
However, we do not want to modify the matrix before we have processed all the necessary information.
* What if we store this in the `matrix[i][0]` or `matrix[0][j]` values?
  * We still need to keep some sort of flag/indicator of whether or not we need to zero-out the 0-th row/column, but we can do that in constant space (i.e. 2 flags)
* We'll run through the matrix twice
  * Initially setting our flags of whether or not the 0-th row/columns need to be zeroed-out
  * Initially checking all the non-`matrix[i][0]` and non-`matrix[0][j]` values to get the information we need
    * We store this information in the corresponding `matrix[i][0]` and `matrix[0][j]` cells
    * After iterating through all the non-`matrix[i][0]` and non-`matrix[0][j]` values, we can then apply the zeroing out process
  * After handling all the elements, we can then zero-out the 0-th row/column if our flags deem it necessary

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
      """
      Do not return anything, modify matrix in-place instead.
      """

      height = len(matrix)
      if height == 0:
        return

      width = len(matrix[0])
      if width == 0:
        return

      left_column_all_zero, top_row_all_zero = False, False
      for i in range(height):
        if matrix[i][0] == 0:
          left_column_all_zero = True

      for j in range(width):
        if matrix[0][j] == 0:
          top_row_all_zero = True

      for i in range(1, height):
        for j in range(1, width):
          if matrix[i][j] == 0:
            matrix[0][j] = 0
            matrix[i][0] = 0

      for i in range(1, height):
        if matrix[i][0] == 0:
          matrix[i] = [0] * width

      for j in range(width):
        if matrix[0][j] == 0:
          for i in range(height):
            matrix[i][j] = 0

      if left_column_all_zero:
        for i in range(height):
          matrix[i][0] = 0

      if top_row_all_zero:
        matrix[0] = [0] * width

```

T = O(n) where n is the size of the matrix  
S = O(1)
