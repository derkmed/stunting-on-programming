# Longest Common Prefix

<b>Question: Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".
  
Example 1:  
Input: ["flower", "flow", "flight"]  
Output: "fl"  
</b>

Thought Process:
* Iterate through the same index for all characters. Stop once we find a character that is not as expected.
* To optimize: we know the upper bound for prefix length is the shortest String!

```python
class Solution:
  def longestCommonPrefix(self, strs: List[str]) -> str:
    if not strs: return ""
    shortest = min(strs, key=len)
    for i, c in enumerate(shortest):
      for s in strs:
        if s[i] != c:
          return shortest[:i]
    return shortest
```

T = O(S) but Theta(minLen * n)  
S = O(1)
