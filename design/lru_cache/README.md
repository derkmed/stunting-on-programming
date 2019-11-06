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


Assuming you cannot use an `OrderedDict`, here is another implementation:

```python

class Node:

  def __init__(self, key: int, value: int):
    self.key = key
    self.value = value
    self.prev = None
    self.next = None 

class LRUCache:
 
  def __init__(self, capacity: int):
    self.dict = dict()
    self.capacity = capacity
    self.head = Node(0, 0)
    self.tail = Node(0, 0)
    self.head.next = self.tail
    self.tail.prev = self.head
    
  def get(self, key: int):
    if key in self.dict:
      node = self.dict[key]
      self.removeNode(node)
      self.addNode(node)
      return node.value
    return -1
  
  def put(self, key: int, value: int):
    if key not in self.dict:
      node = Node(key, value)  
      self.dict[key] = node
      self.addNode(node)
      if len(self.dict) > self.capacity:
        removedNode = self.head.next
        self.removeNode(removedNode)
        del self.dict[removedNode.key]
    else:
      node = self.dict[key]
      self.removeNode(node)
      node.value = value
      self.addNode(node)
    return
    
  def removeNode(self, node: Node):
    prev = node.prev
    nxt = node.next
    prev.next = nxt
    nxt.prev = prev   
    
  def addNode(self, node: Node):    
    node.prev = self.tail.prev
    self.tail.prev.next = node
    self.tail.prev = node
    node.next = self.tail
```

Topics = {Design}
