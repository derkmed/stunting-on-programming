# Smallest String With Swaps

Thought Process:
* Union Find can be O(logn) amortized time: If m operations (either union() or find()) are applied to n elements, the total runtime will be O(mlogn).


```python
class UnionFind:
    
    def __init__(self, n):
      self.p = [i for i in range(n)]
      
    def union(self, x: int, y: int):
      self.p[self.find(y)] = self.find(x)
      
    def find(self, x: int) -> int:
      if self.p[x] != x:
        self.p[x] = self.find(self.p[x])
      return self.p[x]

class Solution:
  
  def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
    
    uf = UnionFind(len(s))
    for pair in pairs:
      uf.union(pair[0], pair[1])
      
    chars_for_id = collections.defaultdict(list)
    for i in range(len(s)):
      chars_for_id[uf.find(i)].append(s[i])
      
    for k in chars_for_id.keys():
      chars_for_id[k].sort(reverse=True)
      
      
    return "".join(chars_for_id[uf.find(i)].pop() for i in range(len(s)))
    
    # The above is the same as the following:
    # new_s = ""
    # for i in range(len(s)):
    #   new_s += chars_for_id[uf.find(i)].pop()
    # return new_s    
```
