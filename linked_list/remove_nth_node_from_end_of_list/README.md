# Remove Nth Node From End of Lost

<b>Question: Given a linked list, remove the n-th node from the end of list and return its head.</b>
```
Example:  
Given linked list: 1->2->3->4->5, and n = 2.  
After removing the second node from the end, the linked list becomes 1->2->3->5.
```

Thought Process:
* There are two possible ways to solve this problem:
  1. Pre-pass through the list to determine the `size` and from there you can calculate `size - n` to determine which node to delete.
  2. Initialize two pointers, one fast and one slow, so that one pointer starts `n + 1` steps ahead. Both pointers can advance by 1 until the fast pointer is null/None.
      * Naturally, this will end up with the slow pointer ending at the node before the node to be deleted.
* In both cases, we want to take into consideration the possible edge case in which the node to be deleted is actually the head of the list.
    * This becomes an edge case because the head of the list does not have a node before it (we could add one, however that is not used in the following solution).
  

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
      if not head: return None
      behind_ptr, ahead_ptr = head, head
      for _ in range(n+1): 
        if not ahead_ptr: return head.next # this is a case where a valid n == list_size
        ahead_ptr = ahead_ptr.next

      while ahead_ptr:
        behind_ptr = behind_ptr.next
        ahead_ptr = ahead_ptr.next
      behind_ptr.next = behind_ptr.next.next
      return head
```

T = O(N)  
S = O(1)

Topics = {Linked List, Two Pointers}
