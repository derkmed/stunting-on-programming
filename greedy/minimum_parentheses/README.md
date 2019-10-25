# Minimum Add to Make Parentheses Valid

<b>Question:</b>
<b>Given a string S of '(' and ')' parentheses, we add the minimum number of parentheses ( '(' or ')', and in any positions ) so that the resulting parentheses string is valid.
Formally, a parentheses string is valid if and only if:</b>
* <b>It is the empty string, or</b>
* <b>It can be written as AB (A concatenated with B), where A and B are valid strings, or</b>
* <b>It can be written as (A), where A is a valid string.</b>
* <b>Given a parentheses string, return the minimum number of parentheses we must add to make the resulting string valid.</b>

```
Examples:

Input: "())"
Output: 1

Input: "((("
Output: 3

Input: "()"
Output: 0
```

Thought Process:
* We can use a stack to keep track of parentheses. \
  * We always add a new parenthesis to the stack except for when the top is a `'('` and the element to add is a `')'`, in which case we pop the stack.
  * We return the length of the stack at the end.
  * Unfortunately, this requires O(n) space.
* Instead, we can keep a +/- counter (+1 for `'('` and -1 for `')'`).
  * For every `'('`, we know there needs to be a `')'` and vice versa.
  * Whenever we find an "unopened" `')'`, we simply move on: there should be a `'('` in front of it, but there is not, so this definitely must count towards the result.
  * From the closing end, we also know for every `'('` accounted for, there must be a `')'`, which is why we can return the sum of the counter and number of missing `'('`.

```python
class Solution:

  def minAddToMakeValid(self, S: str) -> int:
    balance, result = 0, 0
    for c in S:
      balance += 1 if c == '(' else -1
      if balance == -1:
        result += 1
        balance += 1
    return 
```

T = O(n)  
S = O(1)  
Topics = {Stack, Greedy}
