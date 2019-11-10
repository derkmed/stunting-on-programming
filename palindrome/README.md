# Valid Palindrome

<b>Question:</b>
<b> Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome. </b>

```
Examples:

Input: "aba"
Output: True

Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

Thought Process: 
* There's a really nice solution in python for this problem.
* We can iterate up to halfway through the array, keeping track of the first index at which its complement index and itself are not equal in values.
* At this point, we know that we either need to take one char away from the left or right. If neither results in a palindrome, return False.
* We can easily bring the space complexity down to O(1), but for clarity purposes we accept the O(n) version.

```python
  def validPalindrome(self, s: str):
    i = 0
    while i < len(s) // 2 and s[i] == s[-(i+1)]: i += 1
    s = s[i:len(s)-i]
    return s[1:] == s[1:][::-1] or s[:-1] == s[:-1][::-1]
```

T = O(n)  
S = O(n)  (The reverse slicing takes up space)

Topics = {String}

