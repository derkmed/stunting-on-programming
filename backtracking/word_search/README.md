# Word Search I
<b>Question: You are given a 2D board and a word, find if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example:    
board =  
[  
&nbsp;&nbsp;['A','B','C','E'],  
&nbsp;&nbsp;['S','F','C','S'],  
&nbsp;&nbsp;['A','D','E','E']  
]  
  
Given word = "ABCCED", return true.  
Given word = "SEE", return true.  
Given word = "ABCB", return false.  

</b>


Thought Process:
* This is a pretty typical example of backtracking. We can run DFS for each coordinate and recurse among all neighbors.
* To prevent revisitng a coordinate, we can override its value temporarily. Just remember to revert it back (in case we backtrack)!

```python
class Solution:
  def exist(self, board: List[List[str]], word: str) -> bool:

    def dfs(board: List[List[str]], i : int, j: int, word: str) -> bool:
      if not word:
        return True
      elif i < 0 or j < 0 or i >= len(board) or j >= len(board[0]): 
        return False
      elif board[i][j] == word[0]:
        tmp = board[i][j]
        board[i][j] = '#'
        result = dfs(board, i-1, j, word[1:]) or dfs(board, i+1, j, word[1:]) or \
            dfs(board, i, j-1, word[1:]) or dfs(board, i, j+1, word[1:])
        board[i][j] = tmp 
        return result 
      else:
        return False


    m = len(board)
    if m == 0:
      return False
    n = len(board[0])
    for j in range(n):
      for i in range(m):
        coord = (i, j)
        if dfs(board, i, j, word):
          return True

    return False        
```

T = O(mn) where m is the length of the word and n is the size of the board  
S = O(m) where m is the length of the word
