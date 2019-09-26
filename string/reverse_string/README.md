# Reverse String II

<b>Question: Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.</b>

```
Example
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

Thought Process:
* Move in strides of `2k` with pointer `i`. reverse `s[i:k]`.

```python
class Solution:
  def reverseStr(self, s: str, k: int) -> str:
    result = list(s)
    for i in range(0, len(s), 2*k):
      result[i:i+k] = reversed(result[i:i+k])
    return ''.join(result)
```

T = O(N)  
S = O(N)  

Topics = {String}
