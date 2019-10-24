# Find words that can be formed by characters

<b>Question:</b>
<b>You are given an array of strings words and a string chars. A string is good if it can be formed by characters from chars (each character can only be used once). Return the sum of lengths of all good strings in words.</b>

```
Example:
Input: words = ["cat","bt","hat","tree"], chars = "atach"
Output: 6
Explanation: 
The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.
```

Thought Process:
* We need the character counts for the provided reference String as well as the character counts for each word.
* We can break this case down to the single word case.
* If the character counts in the reference string are `>=` than the character counts of all the characters in a single word, that word's length should be added to the result.
* In `python`, you can subtract `Counter` objects! If a `Counter` is subtracted from its superset, an empty `Counter` will be returned!
  * For each key, value pair in the minuend Counter (ideally, a subset), the value is subtracted by the corresponding value in the subtrahend Counter (ideally, a superset).
  
```python
from collections import Counter

class Solution:
  def countCharacters(self, words: List[str], chars: str) -> int:
    result = 0
    for word in words:
      if not Counter(word) - Counter(chars):
        result += len(word)
    return result
```

T = O(mn) where m is the maximum String length  
S = O(mn)  

Topics = {Hash Table}

