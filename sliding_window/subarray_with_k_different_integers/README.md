# Subarrays with K Different Integers

<b>Question:</b>

<b>Given an array A of positive integers, call a (contiguous, not necessarily distinct) subarray of A good if the number of different integers in that subarray is exactly K.</b>
<b>(For example, [1,2,3,1,2] has 3 different integers: 1, 2, and 3.)</b>
<b>Return the number of good subarrays of A.</b>

<b>Notes:</b>
* <b>1 <= A.length <= 20000</b>
* <b>1 <= A[i] <= A.length</b>
* <b>1 <= K <= A.length</b>

```
Example:
Input: A = [1,2,1,2,3], K = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

Thought Process:
* Let's keep two sliding windows, both right-aligned for every index.
  * One of the sliding windows will be the largest possible window ending at this index that holds `K` (unique) elements.
  * The other sliding window will be the largest subarray of the original window that holds `<K` (unique) elements.
* The two windows will always be right-aligned, but with varying left boundaries: keep in mind that the sub-window will have a higher left-index.
* Thought Process Example:

  | Data | Index | Large Window | Small Window | Corresponding Solutions | Ignored Solutions | Difference of left bounds |
  |----|----|----|----|----|----|----|
  | [<b><i>1</i></b>, 2, 1, 2, 3] | 0 | [1] | [1] | | | 1 - 1 = 0 |
  | [<b><i>1, 2</i></b>, 1, 2, 3] | 1 | [1, 2] | [2] | [1, 2,] | [1] | 1 - 0 = 0 | 
  | [<b><i>1, 2, 1</i></b>, 2, 3] | 2 | [1, 2, 1] | [1] | [1, 2, 1], [2, 1] | [1] | 2 - 0 = 2 |
  | [<b><i>1, 2, 1, 2</i></b>, 3] | 3 | [1, 2, 1, 2] | [2] | [1, 2, 1, 2], [2, 1, 2], [1, 2] | [2] | 3 - 0 = 3 |
  | [1, 2, 1, <b><i>2, 3</i></b>] | 4 | [2, 3] | [3] | [1, 2, 3] | [3] | 2 - 1 = 1 |

  * \# solutions = 7
  * What makes this confusing is the indices are zero-based. If you are still confused, try converting this to one-based and it will become clearer.
  
```python
class Window:

  def __init__(self):
    self.char_counts = collections.defaultdict(int)
    self.unique_count = 0
    
  def add(self, x: int):
    if self.char_counts[x] == 0: self.unique_count += 1
    self.char_counts[x] += 1
    
  def remove(self, x: int):
    if x not in self.char_counts: return
    self.char_counts[x] -= 1
    if self.char_counts[x] == 0: self.unique_count -= 1

      
class Solution:

  def subarraysWithKDistinct(self, A: List[int], K: int) -> int:
    result = 0
    
    kWindow = Window()
    sub_kWindow = Window()
    kWindow_left, sub_kWindow_left = 0, 0
    for right, x in enumerate(A):
      kWindow.add(x)
      sub_kWindow.add(x)
      
      while kWindow.unique_count > K:
        kWindow.remove(A[kWindow_left])
        kWindow_left += 1
           
      while sub_kWindow.unique_count >= K:
        sub_kWindow.remove(A[sub_kWindow_left])
        sub_kWindow_left += 1
        
      # Both windows are right-aligned, but sub_k is a subset with a larger left bound index
      result += sub_kWindow_left - kWindow_left
      
    return result
```

T = O(n)  
S = O(n)  

Topics = {Hash Table, Two Pointers, Sliding Window}
