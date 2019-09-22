# Integer to Roman

<b>Question: Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.</b>
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
<b>For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.  
Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:</b>
* I can be placed before V (5) and X (10) to make 4 and 9. 
* X can be placed before L (50) and C (100) to make 40 and 90. 
* C can be placed before D (500) and M (1000) to make 400 and 900.
* Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.
</b>

Thought Process:
* Similar to any form of digit processing, greedy can suffice.
* We just need a mapping from the Roman numeral character to its respective value. 
  * However, we want to keep track of the six possible edge cases: (IV, IX, XL, XC, CD, CM)
* Keep in mind though, that a character can occur multiple times e.g. (MM is entirely valid, just like M)
  * Let's greedily iterate through each possible roman numeral and take as many as possible from the provided `num` (decrement it after taking from it)!
```python
class Solution:
class Solution:
    def intToRoman(self, num: int) -> str:
      roman_numeral = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I' ]
      roman_value = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
      result = ""
      for i, c in enumerate(roman_numeral):
        result += c * int(num // roman_value[i])
        num -= roman_value[i] * (num//roman_value[i])
      return result
```
T = O(N)  
S = O(N)  (for the translation arrays)  

Topcs = {Math, String}
