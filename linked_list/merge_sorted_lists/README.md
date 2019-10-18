# Merge Two Sorted Lists
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
      if not l1: return l2
      if not l2: return l1
      if l1.val < l2.val:
        l1.next = self.mergeTwoLists(l1.next, l2)
        return l1
      else:
        l2.next = self.mergeTwoLists(l1, l2.next)
        return l2
```

T = O(n)  
S = O(n) for stack frames

# Merge K Sorted Lists

Thought Process:
* Use a priority queue to choose the next element to add to the list.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

from queue import PriorityQueue

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
      
      class WrappedListNode():
        def __init__(self, node):
            self.node = node
        def __lt__(self, other):
            return self.node.val < other.node.val
          
      q = PriorityQueue()
      for l in lists:
        if l:
          q.put(WrappedListNode(l))
      head = pointer = ListNode(0) 
      while not q.empty():
        node = q.get().node
        pointer.next = node
        pointer = pointer.next
        node = node.next
        if node:
          q.put(WrappedListNode(node))
      
      return head.next
```

T = O(nklogk)  
S = O(n) for stack frames

# Merge K Sorted Lists (Alternative Solution)

Thought Process:
* Using a priority queue takes up space. How about we Divide and Conquer with the merge 2 lists as the base case?
* Now, we don't need to allocate extra space for another data structure!
* Recursion depth is log(k)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
        
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1: return l2
        if not l2: return l1
        if l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2   
    
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        
        def _mergeKListHelper(l: int, r: int) -> ListNode:
            if l > r: return None
            if l == r: return lists[l]
            if l == r - 1: return self.mergeTwoLists(lists[l], lists[r])
            
            mid = (l + r) // 2
            return self.mergeTwoLists(_mergeKListHelper(l, mid), _mergeKListHelper(mid + 1, r))
        
        return _mergeKListHelper(0, len(lists) - 1)
```

T = O(nlogk)  
S = O(1) but O(n) if you include Stack frames

Topics: {Sort, Divide-And-Conquer}
