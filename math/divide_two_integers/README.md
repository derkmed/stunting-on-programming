# Divide Two Integers

<b>Question: Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator. Return the quotient after dividing dividend by divisor. The integer division should truncate toward zero.</b>

<b>Note:</b>
* <b>Both dividend and divisor will be 32-bit signed integers.</b>
* <b>The divisor will never be 0.</b>
* <b>Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2<sup>31</sup>,  2<sup>31</sup> − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.</b>

```
Example  
Input: dividend = 10, divisor = 3
Output: 3
```

Thought Process:
* We need to assume that the system can only handle
* Dividing a number will only make it smaller!
  * The only real edge case is when we divide -2<sup>31</sup> by 1: it should be 2<sup>31</sup> due to our restricted bit length

```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
       # We must assume a system of 32 bits max
      if (dividend == -2**31 and divisor == -1): return 2**31-1 

      is_positive = ((dividend < 0) == (divisor < 0))
      dividend, divisor = abs(dividend), abs(divisor)
      result = 0
      for i in range(31, -1, -1):
        if dividend >= (divisor << i):
          result += (1 << i)
          dividend -= (divisor << i)
             
      result = result if is_positive else -result
      return result
```
T = O(1)  
S = O(1)  
