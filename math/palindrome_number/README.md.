# Palindrome Number

<b>Question: Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.</b>

Thought Process:
* Take half the digits in reversed order (`y`) and compare with the other half (modified `x`)
  * Make sure to handle the odd # digits case
* Keep in mind that the number of digits to represent a number is equal to `log(x, 10)`

```python
from math import log10
class Solution:
  def isPalindrome(self, x: int) -> bool:
    if x < 0: return False
    elif x == 0: return True
    
    n, y = int(log10(x) + 1), 0
    for _ in range(int(n/2)):
      y = y * 10 + x % 10
      x //= 10
    if n % 2 != 0: x //= 10
    return x == y
```
T = O(logn/log(10)) = O(logn)  
S = O(1)
