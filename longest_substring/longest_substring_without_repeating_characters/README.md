# Longest Substring Without Repeating Characters

<b>Question: Given a string, find the length of the longest substring without repeating characters. 

Input: "abcabcbb"  
Output: 3  
Explanation: The answer is "abc", with the length of 3.  
</b>

Thought Process:
* Keep a window w/`left`+`right` pointers and a dictionary lookup table from seen characters and their corresponding indices
* Whenever we come across a character we've already seen, `left` can jump to the location of that character's mapped index + 1 (the next index)

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
      seen = {}
      max_length, l = 0, 0
      for r, v in enumerate(s):
        if v in seen:
          l = max(l, seen[v] + 1)
        max_length = max(max_length, r - l + 1)
        seen[v] = r
      return max_length
```

T = O(n)
S = O(n)
