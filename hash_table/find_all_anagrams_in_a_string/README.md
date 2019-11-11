# Find All Anagrams in a String

<b>Question:</b>

<b> Given a string s and a non-empty string p, find all the start indices of p's anagrams in s. </b>  
  
<b> Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.</b>  

<b>The order of output does not matter.</b>  

 
```
Example:
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```
```
Example:
Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

Thought Process:
* Approach this with the "sliding window" mindset.
* Start by creating a hash table of required counts for each character in p.
* Use a `set()` to keep track of characters that require a presence within any `len(p)` substring anagram.
* We will slide through `s` in `len(p)` chunks, inching left-to-right a single index at a time
  * Whenever a character (that we care about) is added (from the right) to our `len(p)` window, decrement its count. If the count reaches 0, we can remove it from the required set <i>temporarily</i>.
  * Whenever a character (that we care about) is removed (from the left) of our `len(p)` window, increment its count. If the count is no longer nonzero and once was, the character is once again required and must be added to the required set <i>temporarily</i>.
* Edge cases: 
  * empty strings
  * when `p` is longer than `s`
* Cleaner and easier to reason if we create nested `_decrement()` and `increment()` functions.
  
```python
class Solution:
  def findAnagrams(self, s: str, p: str) -> List[int]:
    # Edge case to consider
    if len(p) > len(s): return []
    
    p_counts = collections.Counter(p)
    required_chars = set(p)
    
    def _decrement(c: str):
      nonlocal p_counts
      if c in p_counts:
        p_counts[c] -= 1
        if p_counts[c] == 0: required_chars.remove(c)
        
    def _increment(c: str):
      nonlocal p_counts
      if c in p_counts:
        p_counts[c] += 1
        if p_counts[c] == 1: required_chars.add(c)
    
    # Set up the initial window
    for i in range(0, len(p)-1):      
      _decrement(s[i])
    
    # Now we simply iterate through the len(s)-len(p) possible end/start indices
    result = []
    for i_end in range(len(p)-1, len(s)):
      _decrement(s[i_end])
      if not required_chars: 
        result.append(i_end - len(p) + 1)  
      _increment(s[i_end - len(p) + 1])
    return result
```

T = O(n)  
S = O(n)  

Topics = {Hash Table} 
