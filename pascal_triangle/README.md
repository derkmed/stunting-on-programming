# Pascal's Triangle I 

### Question: Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.
```
class Solution:
    def generate(self, num_rows: int) -> List[List[int]]:
      triangle = [[1]]
      for i in range(1, num_rows):
        last_row = triangle[-1]
        triangle.append([x + y for x, y in zip(last_row + [0], [0] + last_row)])  
      return triangle[:num_rows]
```
