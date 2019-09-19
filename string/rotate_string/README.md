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
S = O(n) (Can be O(1) if we have a helper function to compare for B[i:]+B[i:] as opposed to building a new String)


<b>A more advanced implementation:</b>

Thought Process:
* This approach employs a KMP-like approach.
* We first generate a `shifts` array. Every element `i` of `shifts` indicates how much `i` must be shifted over such that we find the ending index of the longest prefix that is also a suffix.
  * For instance, in sub/string "aabaa", we can see that `shifts[4] = 3` because the longest prefix-suffix is `aa`.
* Using this `shifts` array, we can then run linear substring matching of `B` in `A+A` because we know that `A+A` must contain `B` if `B` is a valid rotation of `A`.

```python
class Solution:
  def rotateString(self, A: str, B: str) -> bool:
    def generate_shift_table(s: str) -> List[str]:
      shifts = [1] * (len(s) + 1)
      l = -1
      for r in range(len(s)):
        while l >= 0 and s[l] != s[r]:
          l -= shifts[l]
        shifts[r+1] = r - l 
        l += 1
      return shifts        
      
    if len(A) != len(B): return False
    if not A and not B: return True
    
    shifts = generate_shift_table(B)
    match_len = 0
    for char in A+A:
      while match_len >= 0 and B[match_len] != char:
        match_len -= shifts[match_len]
      match_len += 1
      if match_len == len(B):
        return True
    return False
```

T = O(n)  
S = O(n)  
