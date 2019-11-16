# Is Graph Bipartite?

<b>Question:</b>

<b>Given an undirected graph, return true if and only if it is bipartite. Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B. </b>
<b>The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists.  Each node is an integer between 0 and graph.length - 1.  There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.</b>

```
Example:
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.
```

Thought Process:
* Think of what we need from a bipartite graph: because every edge must have one vertex in one subset and its other vertex in the other, we can think of this problem as a coloring problem.
* How about we pick a node and assign it one of two colors.
  * We then can run DFS on all its connected edges and assign colors in an alternating scheme.
  * If it turns out we reach a node we've visited before and the color matches one of its neighbors, we can return False.
  
  
```python
class Solution(object):
  def isBipartite(self, graph):
    color = {}
    
    # This is needed for disconnected groups/forests of nodes
    for node in range(len(graph)):
      if node in color: continue
      
      # Assign an initial coloring to this node
      stack = [node]
      color[node] = 0
      
      # Assign alternate coloring for node's neighbors and DFS 
      while stack:
        curr = stack.pop()
        for neighbor in graph[curr]:
          if neighbor in color and color[neighbor] == color[curr]:
            return False
          if neighbor not in color:
            color[neighbor] = color[curr] ^ 1
            stack.append(neighbor)
    return True
```

T = O(n)  
S = O(n)  

Topics = {Stack, DFS, Graph}
