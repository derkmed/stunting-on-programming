# Campus Bikes

Thought Process:
* Observe that we can have significantly more bikes than workers.
* Start by getting the cross product of the two sets.
* Construct a min-heap on distance between each bike and each worker (make sure to include the index).
* While iterating through the heap, assign unassigned bikes (keep track of assignments via set) to unassigned workers (also keep track of assignments via set).

```python
import itertools
class Solution:
  def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> List[int]:
   
    def _dist(p1: List[int], p2: List[int]) -> int:
      return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    
    bikes_x_workers = itertools.product([i for i in range(len(bikes))], [j for j in range(len(workers))])
    dist_heap = [(_dist(bikes[bi], workers[wi]), bi, wi) for bi, wi in bikes_x_workers]
    heapq.heapify(dist_heap)
    assignments = [-1 for _ in workers]
    assigned_bikes = set()
    assigned_workers = set()

    while len(assigned_workers) < len(workers):
      _, bi, wi = heapq.heappop(dist_heap)
      if bi not in assigned_bikes and assignments[wi] < 0:
        assignments[wi] = bi 
        assigned_bikes.add(bi)
        assigned_workers.add(wi)
    return assignments 
```

Time: O(mn + log(mn))
