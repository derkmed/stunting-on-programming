# Rotate String

<b>Question:
We are given two strings, A and B. A shift on A consists of taking string A and moving the leftmost character to the rightmost position. For example, if A = 'abcde', then it will be 'bcdea' after one shift on A. Return True if and only if A can become B after some number of shifts on A.
</b>

```
Example 1:
Input: A = 'abcde', B='cdeab'
Output: True

Example 2:
Input: A = 'abcde', B = 'abced'
Output: False
```

Thought Process:
* A and B must be the same length
* We can think of the rotation more as a <i>shift</i>
* Once we find the index `i` at which the 0-index has shifted to, we can check to see if B[i:]+B[:i] == A
  * If we wanted to, we could optimize this in such a way that doesn't build a new String

```python
class Solution:
  def rotateString(self, A: str, B: str) -> bool:
    if not A and not B: return True
    if len(A) != len(B): return False
    for i in range(len(B)):
      if B[i] == A[0] and B[i:]+B[:i] == A: # The second condition here can get optimized to not create a new String
        return True

    return False            
```

T = O(n<sup>2</sup>)
S = O(n) (Can be O(1) if we have a helper function to compare for B[i:]+B[i:] as opposed to building a new String
