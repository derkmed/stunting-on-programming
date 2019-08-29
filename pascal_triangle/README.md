# Pascal's Triangle I 

<b>Question: Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.</b>

Thought Process
* Need to keep a list of lists. 
* We definitely want to handle the case in which `num_rows = 0`.
    * Notice that if we forget about this case for a bit, a pattern sort of emerges.
    * From rows 1,2,...n: `current_row = [x + y, for x, y in zip(prev_row + [0], [0] + prev_row)]`.

So, let's initialize our triangle with the first row (`num_rows=1`) and append each new row iterating from `[1, num_rows)`. At the very end, we'll handle the `num_rows = 0` case via slicing with `triangle[:num_rows]`.
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

<b>Question: Given a non-negative index k where k ≤ 33, return the kth index row of the Pascal's triangle. (Note that the row index starts from 0)</b>

Thought Process
* Similar to the above. However, now we only need to keep one row at a time: This time though, it is easier because we know that "0" refers to the first row!
```
class Solution:
    def getRow(self, row_index: int) -> List[int]:
      row = [1]
      for _ in range(row_index):
        row = [x + y for x,y in zip([0] + row, row + [0])]
      return row  

```
