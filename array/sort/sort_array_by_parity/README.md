# Sort Array By Parity II

<b>Question:</b>
<b>
Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even. Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.
You may return any answer array that satisfies this condition. </b>

```
Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
```

Thought Process
* One approach is to keep two pointers:
  1. A pointer to the most recent even index.
  2. A pointer to the most recent odd index.
* Even pointer can be incremented by 2 while it points to an even value.
* Odd pointer can be incremented by 2 while it points to an odd value.
* Whenever the even pointer does not have an even value and the odd pointer also does not have an odd value, we must swap them.

```python
class Solution:
  def sortArrayByParityII(self, nums: List[int]) -> List[int]:
    i, j = 0, 1
    
    while i < len(nums) and j < len(nums):
      if nums[i] % 2 == 0:
        i += 2
      elif nums[j] % 2 != 0:
        j += 2
      else:
        nums[i], nums[j] = nums[j], nums[i]
        i += 2
        j += 2
    return nums
```

T = O(n)  
S = O(1)

Thought Process:
* Another approach involves solely focusing on the even indices.
* Only pass through the even indices while keeping a pointer to the first odd index.
* Whenever we see an odd number at one of the passed even indices, this means this value does not belong here.
  * We want to swap this value with the highest odd index that has an even value!
  * While the odd index pointer is odd, increment it by 2.
  * At this point, the pointers should be set correctly, so swap the two.
  
```python
class Solution:
  def sortArrayByParityII(self, nums: List[int]) -> List[int]:
  '''
  Check each even index. If the value @ the index is odd, swap it with the
  last odd index that holds an even value!
  '''
  j = 1
    for i in range(0, len(nums), 2): # check each even index
      if nums[i] % 2 != 0:    
        while nums[j] % 2 != 0:
          j += 2
        nums[i], nums[j] = nums[j], nums[i]
    return nums
```

T = O(n)  
S = O(1)

Topics = {Array}
