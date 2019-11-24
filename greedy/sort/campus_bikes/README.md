# Campus Bikes

<b>Question:</b>
<br><br>
<b>On a campus represented as a 2D grid, there are N workers and M bikes, with N <= M. Each worker and bike is a 2D coordinate on this grid.</b>
<br><br>
<b>Our goal is to assign a bike to each worker. Among the available bikes and workers, we choose the (worker, bike) pair with the shortest Manhattan distance between each other, and assign the bike to that worker. (If there are multiple (worker, bike) pairs with the same shortest Manhattan distance, we choose the pair with the smallest worker index; if there are multiple ways to do that, we choose the pair with the smallest bike index). We repeat this process until there are no available workers.</b>
<br><br>
<b>The Manhattan distance between two points p1 and p2 is Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|.</b>
<br><br>
<b>Return a vector ans of length N, where ans[i] is the index (0-indexed) of the bike that the i-th worker is assigned to.</b>
<br><br>
<b>Notes:</b>
  * <b>`0 <= workers[i][j], bikes[i][j] < 1000`</b>
  * <b>All worker and bike locations are distinct.</b>
  * <b>`1 <= workers.length <= bikes.length <= 1000`</b>

## Heap Approach
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

T = O(mn + mnlog(mn))
S = O(mn)  
<br>

## Bucket Approach
Thought Process:
* We would like to do better than O(mnlog(mn)). 
* We definitely need to evaluate all pairs inherently. Otherwise, how else can we compare pairs?
* Notice that we are given a constraint on the coordinates: `0 <= workers[i][j], bikes[i][j] < 1000`: the maximum distance between points is a <i>finite</i> number! Thus, distances are within the bounds of `0 <= dist <= 2000`.
* What if we build a list of 2001 buckets (`0` to `2000`) where each bucket index holds a list of pairs that have that distance between them?
  * The list of pairs can be arranged in increasing order of worker indices.

```python
import itertools
class Solution:
  def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> List[int]:
   
    MAX_DIST = 2001
  
    def _dist(p1: List[int], p2: List[int]) -> int:
      return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])
    
    # observe: the maximum distance is 2000!
    bike_indices, worker_indices = [bi for bi in range(len(bikes))], [wi for wi in range(len(workers))]
    index_pairs_iter = itertools.product(worker_indices, bike_indices) # index_pairs_iter will naturally place the lower-worker pair first!
    
    # generate buckets of pairs
    pair_for_distance = [[] for _ in range(MAX_DIST)]
    for index_pair in index_pairs_iter:
      wi, bi = index_pair
      pair_for_distance[_dist(bikes[bi], workers[wi])].append((wi, bi))
      
    # assign workers to bicycles
    worker_assignments = [-1 for _ in range(len(workers))]
    assigned_bikes, assigned_workers = set(), set()
    for pairs_with_dist in pair_for_distance:
      for pair in pairs_with_dist:        
        wi, bi = pair
        if bi not in assigned_bikes and wi not in assigned_workers:
          worker_assignments[wi] = bi
          assigned_bikes.add(bi)
          assigned_workers.add(wi)
        # we can probably break out of this loop here early, but it is omitted for readability

    return worker_assignments
```

T = O(mn)  
S = O(mn)  

Topics = {Greedy, Sort}
