# K Closest Points To Origin

<b>Question:</b>
<b>We have a list of points on the plane.  Find the K closest points to the origin (0, 0).</b>
<b>(Here, the distance between two points on a plane is the Euclidean distance.)</b>
<b>You may return the answer in any order.  The answer is guaranteed to be unique (except for the order that it is in.)</b>

```
Example
Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
Explanation: 
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest K = 1 points from the origin, so the answer is just [[-2,2]].
```

Thought Process:
* Sort the elements and take the top K smallest.
    
```python
class Solution:
  def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:

    # Helper function to calculate a point's distance from the origin
    # SqrRt is unnecessary because it is increasingly monotonic
    def dist(i: int): return points[i][0]**2 + points[i][1]**2
    
    sorted_pts = sorted(enumerate(points), key=lambda index_pt_pair: dist(index_pt_pair[0]))
    return list(map(lambda index_pt_pair: index_pt_pair[1], sorted_pts))[:K]
```

T = O(nlogn)  
S = O(1)  

Thought Process:
* Use a max-heap and constrain its size to be `K`.

```python
import heapq
class Solution:
  def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:

    # Helper function to calculate a point's distance from the origin
    # SqrRt is unnecessary because it is increasingly monotonic
    def dist(i: int): return points[i][0]**2 + points[i][1]**2

    max_heap = []
    for i in range(len(points)):
      point_dist = dist(i)
      heapq.heappush(max_heap, (-point_dist, points[i]))
      if len(max_heap) > K: heapq.heappop(max_heap)
    return map(lambda tup: tup[1], max_heap)
```

T = O(nlogk)  
S = O(n)  


Topics = {Sort Heap}
