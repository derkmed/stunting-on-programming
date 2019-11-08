# Replace With Rank

<b>Question: Given an array of distinct integers, we need to replace each element of the array with its rank. The minimum value element will have the highest rank.</b>

Thought Process:
* Each element has an index in the range of 1 to n.
* Let's map the element to its corresponding index (can use either a dict or a list of `(element, index)` tuples) and sort by the value at eachdex.
* Using this sorted list, we now can rank each index with the new value that should be at index (`+1` of course).

```python
class Solution: 
  def replaceWithRank(self, arr: List[int]) -> List[int]:
    index_sorted_on_value = sorted([i for i in range(len(arr))], key=lambda i: arr[i])
    index_to_rank = {index: i+1 for i, index in enumerate(index_sorted_on_value)}
    return  [index_to_rank[i] for i in range(len(arr))]   
```

T = O(nlogn)  
S = O(n)  

Topics = {Array, Sort (By Value At Index)}  
