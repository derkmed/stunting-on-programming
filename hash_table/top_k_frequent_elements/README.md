# Top K Frequent Elements

<b>Question: Given a non-empty array of integers, return the k most frequent elements.</b>

```
Example 1:
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

Thought Process:
* We need the frequencies for each numerical value.
* Once we have these frequencies, maybe we can build a heap to choose the top K frequencies and their corresponding values?

```python
class Solution:
  def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    
    numFrequency = collections.Counter(nums)
    
    return heapq.nlargest(k, numFrequency.keys(), key=numFrequency.get)
    # Alternative:
    # return sorted(numFrequency.keys(), reverse=True, key=numFrequency.get)[:k]
```

T = O(nlogk) or O(nlogn)
S = O(n)

Topics = {Hash Table, Heap}

Thought Process (Elite):
* How about we produce buckets and assign number values to a bucket based off of their frequencies?
* We can then `chain` the unpacked buckets and assuming we have assigned numbers to buckets in decreasing frequency from left-to-right, we should have values in declining frequency.

```python
class Solution:
  def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    buckets = [[] for _ in nums]
    numFrequency = collections.Counter(nums)
    for num, freq in numFrequency.items(): buckets[-freq].append(num)
    return list(itertools.chain(*buckets))[:k]
```
T = O(n)  
S = O(n)  
