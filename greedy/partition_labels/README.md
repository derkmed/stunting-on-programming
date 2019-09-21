# Partition Labels

<b>Question: A string S of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.</b>

```
Example:   
Input: S = "ababcbacadefegdehijhklij"  
Output: [9,7,8]  
Explanation:  
The partition is "ababcbaca", "defegde", "hijhklij".  
This is a partition so that each letter appears in at most one part. A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

Thought Process:
* We can solve this greedily.
* First we need to obtain the last occurring index of each character.
* Then, we can iterate through the array linearly and just keep track of the maximum ending occurring index of each character we see.
  * When the current index is equal to the maximum ending occurring index of all characters seen in the "current segment", we know this marks the end of a partition.

```python
class Solution:
  def partitionLabels(self, S: str) -> List[int]:
    last_occurrence = {c: i for i, c in enumerate(S)}        
    end_of_curr_segment = 0
    result = []
    for i, c in enumerate(S):
      end_of_curr_segment = max(end_of_curr_segment, last_occurrence[c])
      if i == end_of_curr_segment:
        result.append(end_of_curr_segment - i + 1)
        end_of_curr_segment = i + 1
    return result     
```

T = O(N)  
S = O(N) (however, knowing that our alphabet is constrained, we could say O(1) because there are only 26 lowercase alphabet characters)
