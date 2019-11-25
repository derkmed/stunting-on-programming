# Campus Bikes II

<b>Question:</b>
<br><br>
<b>
On a campus represented as a 2D grid, there are N workers and M bikes, with N <= M. Each worker and bike is a 2D coordinate on this grid.
</b><br>
<b>
We assign one unique bike to each worker so that the sum of the Manhattan distances between each worker and their assigned bike is minimized.
</b><br>
<b>
The Manhattan distance between two points p1 and p2 is Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|.
</b><br>
<b>
Return the minimum possible sum of Manhattan distances between each worker and their assigned bike.
</b>
<br> 

Thought Process:
* We need to do an exhaustive search. 
* One way to think about this problem is to think of it as a graph traversal. 
* There are `w` workers. Let's assign each  a bike accordingly.
  * Imagine a graph. For each `w` workers and `b` bikes, there are `w*b` nodes. 
  * We can initialize a root node.
  * Each node represents a state: the `i`th worker has been assigned to the `j`th bike. 
  * Each edge will represent the distance cost incurred visiting each node/state (the cost of assigning the `i`th worker to the `j`th bike.
  * The final states should connote reaching any node corresponding to the final worker. They will inherently be `w` edges away from the root node.
* However, care must be taken in regard to visited states i.e. multiple workers cannot share the same bicycle assignment!
  * We'll use a `visited` bit array to indicate previous assignments applied to get to the current node/state.
* We can run Dijkstra's to get the shortest path to any final state. The shortest path will correspond to the minimum distance sum.


```python

```
