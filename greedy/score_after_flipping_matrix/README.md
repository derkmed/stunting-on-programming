# Score After Flipping Matrix

<b>Question:</b>

<b>We have a two dimensional matrix A where each value is 0 or 1. A move consists of choosing any row or column, and toggling each value in that row or column: changing all 0s to 1s, and all 1s to 0s.
After making any number of moves, every row of this matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.
Return the highest possible score.</b>

```
Input: [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation:
Toggled to [[1,1,1,1],[1,0,0,1],[1,1,1,1]].
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```

Thought Process:
* A `1` in the ith column from the right contributes 2<sup>i</sup> to the score.
* Naturally, we want to maximize the leftmost digit because 2<sup>n</sup> > 2<sup>n-1</sup> + 2<sup>n-2</sup> + ... 2<sup>0</sup>
* Thus, the rows need to be toggled in such a way that the left-most column is all `0` (with the goal of toggling the whole column to `1`) or `1`.
* We can easily toggle the first column to all 0 via XOR-ing by itself as a representation of setting all to 0.
  * However, we'd have to apply this XOR to all other columns as well.
  * Luckily, for the other columns, we can "flip" their values to maximize the column sum i.e. the` maximum column_sum = max(column_sum, n - column_sum)`
  
```python
class Solution:
  
  def matrixScore(self, A: List[List[int]]) -> int:
      m, n = len(A), len(A[0])
      result = 0
      for c in range(n):
        col_sum = 0
        for r in range(m):
          col_sum += A[r][c] ^ A[r][0]
        result += max(col_sum, m - col_sum) * 2 ** (n - 1 - c)
      return result
```
T = O(rc) (r being number of rows and c being number of columns)  
S = O(1)  

Topics = {Greedy}
