# Longest Palindromic Substring
<b>Question: Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.</b>

```
Exmaple    
Input: "babad"  
Output: "bab"  
Note: "aba" is also a valid answer
```

Thought Process:
* Every palindrome has a center where it divides from. 
  * For odd length palindromes, this center is a single character.
  * For even length palindromes, this center is between two similar characters.
* What if we iterate through each index of the provided String and compare palindrome lengths from considering it as the center or the window between it and the next character as the center?
* There does exist a DP-solution, but it produces O(n<sup>2</sup>) space and time complexity

```python
class Solution:
  def longestPalindrome(self, s: str) -> str:
    def max_palindrome_length_from_center(s: str, i: int, j: int) -> int:
      if not s: return 0
      while i >= 0 and j < len(s) and s[i] == s[j]:
        i -= 1
        j += 1
      return j - i - 1
      
    if not s: return ""
    result_start, result_end = 0, 0
    for i in range(len(s)):
      s_odd = max_palindrome_length_from_center(s, i, i)
      s_even = max_palindrome_length_from_center(s, i, i+1)
      candidate_palindrome_length = max(s_odd, s_even)
      if candidate_palindrome_length > result_end - result_start + 1:
        result_start = i - (candidate_palindrome_length - 1) // 2
        result_end = i + candidate_palindrome_length // 2
    return s[result_start:result_end+1]
```


T = O(n<sup>2</sup>)  
S = O(n) (for the new String, but try to avoid saving temporary substring variables to reduce used space)
Topics = {String}
