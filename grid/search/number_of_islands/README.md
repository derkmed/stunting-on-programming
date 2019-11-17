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

Thought Process:
* Alternatively, you can try bfs, which theoretically should have a lower space complexity because of the nature of BFS and how all elements it processes at once are supposedly the "same degree" away from the starting node/location.

```python
class Solution:
  def numIslands(self, grid: List[List[str]]) -> int:
    
    def _is_valid(i: int, j: int):
      return 0 <= i < len(grid) and 0 <= j < len(grid[0])
    
    def _bfs(i: int, j: int):
      q = collections.deque([(i, j)])
      grid[i][j] = '0'
      while len(q) > 0:
        x, y = q.popleft()        
        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
          if _is_valid(x+dx, y+dy) and grid[x+dx][y+dy] == '1': 
            q.append((x+dx, y+dy))
            grid[x+dx][y+dy] = '0'
    
    result = 0    
    for i in range(len(grid)):
      for j in range(len(grid[0])):
        if grid[i][j] != '1':  continue         
        _bfs(i, j)
        result += 1
    return result
```

T = O(mn)  
S = O(min(m,n))  

Topics = {Grid, DFS, BFS}
