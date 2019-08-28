# Pascal's Triangle I 

##### Question: Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.

Keep a list of lists. We definitely want to handle the case in which `num_rows = 0`, but notice that if we forget about this case for a bit, a pattern sort of emerges from rows 1,2,...n: `current_row = [x + y, for x, y in zip(prev_row + [0], [0] + prev_row)]`.

We'll initialize our triangle with the first row (`num_rows=1`) and append each new row. At the very end, we'll handle the `num_rows = 0` via slicing with `triangle[:num_rows]`.
```
class Solution:
    def generate(self, num_rows: int) -> List[List[int]]:
      triangle = [[1]]
      for i in range(1, num_rows):
        last_row = triangle[-1]
        triangle.append([x + y for x, y in zip(last_row + [0], [0] + last_row)])  
      return triangle[:num_rows]
```

# Pascal's Triangle II

#####
