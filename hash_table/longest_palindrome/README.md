# Longest Palindrome

<b>Question: Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.</b>

<b>Note:</b>
* <b>Assume the length of given string will not exceed 1,010.</b>
* <b>This is case sensitive, for example "Aa" is not considered a palindrome here.</b>

Thought Process:
* Let's keep a hash table or dictionary of character counts.
* We can build a palindrome from some valid subset of the characters we've seen.
* For every character we select however, there must be another instance of that character to mirror itself for us to construct a valid palindrome.
  * This is true for all characters we select to build the palindrome except for 1, the center.


```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        d = collections.Counter()
        for c in s: d[c] += 1
        
        result = 0
        for c in list(d.keys()):
            count_unique_palindrome_char = (result % 2 == 0 and d[c] % 2 != 0)
            result += d[c] // 2 * 2 + 1 if count_unique_palindrome_char else d[c] // 2 * 2
        return result
```

T = O(n)   
S = O(n)  

Topics = {Hash Table}
