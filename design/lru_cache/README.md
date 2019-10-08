# LRU Cache

<b>Question: Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.</b>  

<b>get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.</b>    
<b>put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.</b>    
<b>The cache is initialized with a positive capacity.</b>

Thought Process:
* Double-linked list (nodes hold key + value pair) w/ head + tail references and a constant-time-lookup mapping between keys and respective nodes?
* In python, simply use `OrderedDict`.
  * Use `OrderedDict.popitem(False)` to delete the first element (the `False` is for FIFO).

```python
from collections import OrderedDict

class LRUCache:

  DEFAULT = -1
  
  def __init__(self, capacity: int):
    self.d = OrderedDict()
    self.capacity = capacity

  def get(self, key: int) -> int:
    if key in self.d:
      v = self.d[key]
      self.put(key, v)
      return v
    return self.DEFAULT
            
  def put(self, key: int, value: int) -> None:
    if key in self.d:
      self.d.pop(key)
    elif len(self.d.items()) == self.capacity:
        self.d.popitem(last=False)
    self.d[key] = value
```

T = O(1)  
S = O(n)  


Topics = {Design}
