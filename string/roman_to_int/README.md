<b>Question: Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M. Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.</b>
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

Thought Process:
* First consider the six edge cases: `IV`, `IX`, `XL`, `XC`, `CD`, `CM`.
* If we iterate through the String linearly, we just need to be concerned with the character after the current (if it exists). If it is larger, we must decrement the value of the current Roman Numeral character.

```python
class Solution:
    def romanToInt(self, s: str) -> int:
      roman_value = {'M': 1000, 'D': 500, 'C': 100, 'L': 50, 'X': 10, 'V': 5, 'I': 1}      
      i, result = 0, 0
      for i in range(len(s)):
        should_subtract = (i < len(s) - 1) and (roman_value[s[i]] < roman_value[s[i+1]])
        add_to_result = -roman_value[s[i]] if should_subtract else roman_value[s[i]]
        result += add_to_result
      return result
```

T = O(N)  
S = O(N) (for the mapping)
