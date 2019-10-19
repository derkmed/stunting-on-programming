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

# Letter Case Permutation

<b>Question:</b>
<b>Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.</b>

```
Examples:
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]`
```

Thought Process:
* Think of the number of possible permutations. We should expect 2<sup>#characters</sup>.
* Think of this problem as a tree of decisions in which we start from the base string.
  * For each index of the String that is a valid character, we have the option of branching off into a subtree in which:
    1. the index's character case is switched
    2. the index's character case is preserved
 * Let's a keep a container of `result`s to start at. At each timestep (for each index), we can create a new container of the `next_result`s and expand each result, taking into account the character case transformation discussed previously.

```python
class Solution:
  def letterCasePermutation(self, S: str) -> List[str]:
    result = [S]
    for i, _ in enumerate(S):
    
      next_result = []
      for r in result:
        if r[i] >= 'a' and r[i] <= 'z':
          next_result.append(str(r[:i] + r[i].upper() + r[i+1:]))
        elif r[i] >= 'A' and r[i] <= 'Z':
          next_result.append(str(r[:i] + r[i].lower() + r[i+1:]))
        next_result.append(r)
        
      result = next_result
    return result
```

T = O(2<sup>n</sup>)  
S = O(2<sup>n</sup>)  

