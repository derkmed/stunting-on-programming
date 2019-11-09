# Daily Temperatures

<b>Question:</b>

<b>Given a list of daily temperatures T, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.</b>

<b>For example, given the list of temperatures T = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].</b>

<b>Note: The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].</b>


Thought Process:
* We want to record the difference in index between the <i>next</i> hotter temperature and the current temperature for each element.
* If we iterate through each element, the current temperature when compared with the element prior is either:
  1. larger (`>`)
  2. less than or equal (`<=`)
* If we use a stack, we can easily keep essentially a <i>descending</i> buffer of "unresolved" indices. Whenever we encounter a temperature at a later index that is hotter than the temperature at the head of the stack, we can "resolve" the index at the stack head.

```python
class Solution:
  def dailyTemperatures(self, T: List[int]) -> List[int]:
    stack = [] # stack of indices
    result = [0 for _ in range(len(T))]
    for i, temp in enumerate(T):
      while stack and temp > T[stack[-1]]:
        cooler_temp_index = stack.pop()
        result[cooler_temp_index] = i - cooler_temp_index
      stack.append(i)
                
    return result
```

T = O(n)  
S = O(n)  

Topics = {Stack}
