#

<b>Question:</b>

<b>
You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.

Define a pair (u, v) which consists of one element from the first array and one element from the second array.

Return the k pairs (u1, v1), (u2, v2), ..., (uk, vk) with the smallest sums.
</b>


Though Process:
* Observing the "sorting" element of this problem may indicate there may be an effective solution involving a priority-queue (min-heap).
* Visualizing the problem as an x-y graph in which the axes correspond to the list indices can be super helpful.
** You can start with the (0, 0) coordinate by "evaluating" it and adding its neighbors for the next iteration of evaluation. How do we identify the right neighbors? 
** One way to think about which neighbors to add to the queue could be simply advancing to the next element in either list in such a way that we do not record duplicate pairs
** You can start with the (0, 0) coordinate by "evaluating" it and adding its neighbors for the next iteration of evaluation. How do we identify the right neighbors? 
** One way to think about which neighbors to add to the queue could be simply advancing to the next element in either list in such a way that we do not record duplicate pairs. Given the nature of this though, you'll have to eventually maintain some sort of `set` to filter out already-visited coordinates.
* Because the two lists are sorted, you can do something especially clever that removes the need for an additional `set` for filtering out visited coordinates.


```
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]: 
        '''
        Imagine an x-y grid with the axes corresponding to the nums lists
        We can evaluate a coordinate's "neighbors" by placing them on a priority queue
        and deciding which coordinate to evaluate and add to our results list
        '''
        results = []
        
        # start with the first cell
        q = [(nums1[0] + nums2[0], 0, 0)]
        
        while q and len(results) < k:
            _, x, y = heapq.heappop(q)
            results.append((nums1[x], nums2[y]))
 
            # When adding neighbors, you only need to expand to the 
            # next 'x' when y == 0 because of the sorted nature
            # This also helps obviate the need for maintaining a set for 
            # filtering out "repeated" coordinates to evaluate.ConnectionResetError
            neighbors = [(x, y+1)]
            if y == 0:
                neighbors.append((x+1, y))                               
            
            for n in neighbors:
                i, j = n
                if i >= len(nums1) or j >= len(nums2):
                    continue
                heapq.heappush(q, (nums1[i] + nums2[j], i, j))
                
        return results


```

T = O(min(k, nlogn))
S = O(n^2)


Topics = {Array, Priority Queue}
