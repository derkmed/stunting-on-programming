# Permutations II
<b>Question:</b>
<b>Given a collection of numbers that might contain duplicates, return all possible unique permutations.</b>

```
Example
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```
Thought Process:
* Keep a running container of `results`.
* For each number, try adding it to each result in `results`. The new concatenations should be stored in a separate container that we will overwrite `results` with upon completion.

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result = [[]]
        ''' 
        keep a running container of results. 
        For each number, try adding it to each index of each result in results.
        '''
        for n in nums:
            next_result = []
            for r in result:
                for i in range(len(r)+1):
                    next_result.append(r[:i] + [n] + r[i:])
                    if i < len(r) and n == r[i]: break
            result = next_result
        return result        
```
T = O(n!)   
S = O(n!)  
