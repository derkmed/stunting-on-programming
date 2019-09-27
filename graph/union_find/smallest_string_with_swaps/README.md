# Smallest String With Swaps

```python
class UnionFind:

  def __init__(self, n: int):
    self.p = list(range(n))

  def union(self, x: int, y: int): 
    self.p[self.find(x)] = self.find(y)

  def find(self, x: int):  
    # Recursively follow the parent references
    if x != self.p[x]:
      self.p[x] = self.find(self.p[x])
    return self.p[x]

class Solution:

  def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:

    n = len(s)
    union_find = UnionFind(n)
    for i, j in pairs:
      union_find.union(i, j)

    # Assign Union Find IDs to list of characters
    id_chars = collections.defaultdict(list)
    for i in range(len(union_find.p)):
      id_chars[union_find.find(i)].append(s[i])

    # Sort the list of characters for each ID
    for k in id_chars.keys():
      id_chars[k].sort(reverse=True)

    result = []
    for i in range(n):
      result.append(id_chars[union_find.find(i)].pop())
    return ''.join(result)

```
