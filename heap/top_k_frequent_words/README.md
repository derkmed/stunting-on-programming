# Top k Frequent Words

<b>Question: Given a non-empty list of words, return the k most frequent elements.</b>

<b>Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.</b>

```
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
```

Thought Process:
* We definitely need to keep counts of the element frequencies. This requires us to iterate through every element at least once.
  * Perhaps we can keep some sort of `HashMap`/`Dict` from elements to corresponding counts?
* We can add elements to a max Priority Heap of size `k`, to ensure that we have the top `k` elements.
  * We need to make sure there is alphabetical-order tie-breaking implemented!
  * Luckily, Python heapq supports this inherently with its tuple entries (comparing first then following and so on elements).
  
```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
      counts = collections.Counter()
      for word in words: counts[word] += 1
      
      top_k = []
      for word in counts.keys(): heapq.heappush(top_k, (-counts[word], word))                       
      return [heapq.heappop(top_k)[1] for _ in range(k)]   
```

T = O(nlogk)  
S = O(n)  
  
Topics = {Heap, Hash Table, Trie}  
