# Move Zeroes

<B>Question: Given an array `nums`, write a function to move all `0`'s to the end of it whiel maintaining the relative order of the non-zero elements.</b>

```
Example
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

Thought Process:
* If we divide the array into segments, there are two types of segments to consider:
  1. Segments of contiguous zeroe(s).
  2. Segments of contiguous non-zeroe(s).
* These segments will alternate if they appear. Thus, we can use the two pointer approach to effectively determine where the next two segments of each type appear.
  * When we come across a `0`, its correct position is either its current position or later. Thus, we should save a reference pointer to this index.
  * When we come across a non-`0`, its correct position is either its current position or earlier. Thus, once we reach our first non-zero, we can keep a reference pointer to this index.
* Once we have both reference pointers to the contiguous-`0` segment and a following contiguous-non-`0` segment, we can start swapping values and increment both pointers.
  * This can repeat until we reach the next contiguous-`0` segment, in which we start from the beginning again.

* The following is a very succint implementation of what we've thought about. To shorten the number of lines of code, we can observe that sometimes, the `zero_index` does not necessarily have to point to a 0 value in the array.
  * The `zero_index` mostly represents a "slow" pointer where the current element really belongs, ignoring all zero values.
  * If the `zero_index` is never set (to some index behind the current), values will just get swapped with themselves: nothing wil happen if no contiguous `0`-segment is found.

* Edge cases for when there are non-zero values only? 
  * `[1]`
  * `None`?
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        zero_index = 0
        for i in range(len(nums)):
          if nums[i] != 0: 
            tmp = nums[zero_index]
            nums[zero_index] = nums[i]
            nums[i] = tmp
            zero_index += 1
            
        return nums  
```

T = O(n)  
S = O(1)  

Topics = {Array, Two Pointers}
