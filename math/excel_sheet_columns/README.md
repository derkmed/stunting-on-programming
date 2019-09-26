# Excel Sheet Column Number

<b>Question: Given a column title as appears in an Excel sheet, return its corresponding column number.</b>

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

Thought Process:
* This is like having a base 26 (digits 0-25) number and using the uppercase alphabet to represent each digit.
* How about we handle the least significant digit and add it to a maintained result, and then shift the string over after also shifting the result over (by power of 26) correspondingly?

```python
class Solution:
  def titleToNumber(self, s: str) -> int:
    result = 0
    for i, c in enumerate(reversed(s)):
      result += (ord(c) - ord('A') + 1) * 26**i 
    return result
```

T = O(N)  
S = O(N)  

Topics = {Math}

# Excel Sheet Column Title

<b>Question: Given a positive integer, return its corresponding column title as appear in an Excel sheet.</b>

```
Example:
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```

Thought Process:
* This is like having a base 26 (digits 0-25) number and using the uppercase alphabet to represent each digit.
* How about we handle the least significant digit and add it to a maintained result, and then shift the number over correspondingly (by 26) so we can repeat.

```python
class Solution:
  def convertToTitle(self, n: int) -> str:      
      result = ""  
      while n > 0:
        result = chr(ord('A') + ((n-1) % 26)) + result
        n = (n-1) // 26
      return result
```

T = O(N)  
S = O(N)  
Topics = {Math}
