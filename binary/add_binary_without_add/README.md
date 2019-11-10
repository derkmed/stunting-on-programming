# Add Binary

<b>Question:</b>
<b>Given two binary strings, return their sum (also a binary string). The input strings are both non-empty and contains only characters 1 or 0.</b>

```
Example:
Input: a = "11", b = "1"
Output: "100"
```

Thought Process:
* We cannot use '+'.
* First we should convert the binary strings to integers.
* Typically with binary problems, when you have no idea where to start, consider the `XOR` operation.
* Let's take two example values: `0b1111` and `0b0010`.
  * The answer without the carry will be `0b1111 ^ 0b0010 = 0b1101`.
  * The carry-over will be `0b1111 & 0b0010 << 1 = 0b01010`.
  * Hmmm... well now we just need to add the above two values (recurse as long as there exists a carry-over!)
  
```python
class Solution:
  def addBinary(self, a: str, b: str) -> str:
    x, y = int(a, 2), int(b, 2)
    while y:
      without_carry = x ^ y
      carry = (x & y) << 1
      x, y = without_carry, carry # Keep "adding"
    return bin(x)[2:]
```

T = O(1)  
S = O(1)  

Topics = {Math, String}
