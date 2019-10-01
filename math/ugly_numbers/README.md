# Ugly Number II

<b>Question: Write a program to find the n-th ugly number. Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. </b>
<b>Note:</b>
1. <b>1 is typically treated as an ugly number.</b>
2. <b>n does not exceed 1690.</b>

```
Example:
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

Thought Process:
* The ugly numbers start at 1.
* Numbers can only have 2,3, and 5 as their prime factors.
  * Duplicates: Some of these overlap e.g. 6 has both 2 and 3 as a prime factor.
* Keep a list of the ugly numbers so far, and 3 pointers `i2`, `i3`, and `i5` to keep track of which multiple of `2`,`3`, and `5` we are pointing to.
* Pick the smallest between `2 * ugly[i2]`, `3 * ugly[i3]`, and `5 * ugly[i5]`. And, increment the pointer by +1 for whichever of these 3 values the smallest corresponds to.
   * Make sure though that if two pointers point to effectively the same value (a duplicate), they both get incremented.
   * Having parallel if statements helps here.
   
 ```python
class Solution:
  def nthUglyNumber(self, n: int) -> int:
    ugly = [i]
    i2, i3, i5 = 0, 0, 0
    for _ in range(1, n):
      ugly_2, ugly_3, ugly_5 = ugly[i2] * 2, ugly[i3] * 3, ugly[i5] * 5
      next_smallest = min(ugly_2, ugly_3, ugly_5)
      ugly.append(next_smallest)
      if next_smallest == ugly_2: i2 += 1
      if next_smallest == ugly_3: i3 += 1
      if next_smallest == ugly_5: i5 += 1
    return ugly[-1]
 ```
 
 T = O(n)  
 S = O(n)  
 
 Topics = {Math, Dynamic Programming}
