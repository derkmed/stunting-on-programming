# Add Two Numbers

<b>Question: You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list. You may assume the two numbers do not contain any leading zero, except the number 0 itself.</b>
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

Thought Process:
* Let's create a dummy node that will point to the head of the result.
* For any digit between up to 2 nodes from in the provided lists, get the sum of each node value if existent as well as the `carry_over` from the last digit (initialized to zero).
* Reset the `carry_over` accordingly (for the next sum).
* Construct a new node and move the pointer forward.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
  def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    carry_over = 0
    head = ListNode(-1)
    tmp = head
    while l1 or l2 or carry_over > 0:
      node_val = carry_over + (l1.val if l1 else 0) + (l2.val if l2 else 0)
      carry_over = node_val // 10
      node_val %= 10
      tmp.next = ListNode(node_val)
      tmp = tmp.next
      l1 = l1.next if l1 else l1
      l2  = l2.next if l2 else l2
    return head.next
```

T = O(N)  
S = O(N)  

Topics = {Linked List, Math}
