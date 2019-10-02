# Ugly Number

<b>Question: Write a program to check whether a given number is an ugly number. Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.</b>

```
Example:
Input: 6
Output: true
Explanation: 6 = 2 × 3
```

Thought Process:
* The provided number can be obtained via solely the product of an arbitrary number of `2`'s, `3`'s, and `5`'s.
* We can loop through each factor and keep dividing by that value as many times as possible (it is divisible by the number and decreasing).
* If what we have at the end `==1`, then we can return `True`.

```python
class Solution:
  def isUgly(self, num: int) -> bool:
    for p in [2, 3, 5]:
      while num % p == 0 and num % p < num: num //= p
    return num == 1
```

T = O(n)
S = O(1)

Topics = {Math}
