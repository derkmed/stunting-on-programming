# Number of Islands

<b>Question:</b>  

<b>Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.</b>

```
Example:
Input:
11110
11010
11000
00000

Output: 1
```

Thought Process: 
* We can do a simple dfs traversal and mark visited 1-locations with an `X`.

```python
class Solution:
  def numIslands(self, grid: List[List[str]]) -> int:
    VISITED = 'X'
    result = 0    
    for i in range(len(grid)):
      for j in range(len(grid[0])):
        if grid[i][j] == VISITED or grid[i][j] == '0': 
          continue
        stack = [(i, j)]
        while stack:
          x, y = stack.pop()
          grid[x][y] = VISITED
          for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            if 0 <= x+dx < len(grid) and 0 <= y + dy < len(grid[0]) and \
              grid[x+dx][y+dy] == '1': 
              stack.append((x+dx, y+dy))
        result += 1
    return result        
```

T = O(mn)  
S = O(mn)
