# Valid Parentheses

<b>Question: Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.  
An input string is valid if:</b>  
* <b>Open brackets must be closed by the same type of brackets.</b>
* <b>Open brackets must be closed in the correct order.</b>
* <b>Note that an empty string is also considered valid.</b>

```
Example 1:
Input: "()"
Output: true  
```
```
Example 2:  
Input: "(]"  
Output: false  
```

Thought Process:
* Let's use a stack to keep track of openers and closers
* Whenever an opener is closed properly, we can pop it from the stack!
* A non-empty stack at the end indicates an invalid String!

```python
class Solution:
  def isValid(self, s: str) -> bool:
    closer_for = {'(': ')', '[': ']', '{': '}'}
    opener = set(['(', '[', '{'])
    seen_stack = []
    for c in s:  
      if c in opener:
        seen_stack.append(c)
      elif seen_stack and closer_for[seen_stack[-1]] == c:
        seen_stack.pop()
      else:
        return False          
    return not seen_stack
```
T = O(N)  
S = O(N)  
