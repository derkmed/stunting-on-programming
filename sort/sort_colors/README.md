# Sort Colors

<b>Question:</b>

<b>Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.</b>
<br>
<b>Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.</b>
<br>
<b>Note: You are not suppose to use the library's sort function for this problem.</b>
<br>
<b>A rather straight forward solution is a two-pass algorithm using counting sort.</b>
<b>First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.</b>
<b>Could you come up with a one-pass algorithm using only constant space?</b>

```
Example:
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

Thought Process:
* There are three regions. Let's solely focus on keeping boundaries for the `0` and `2` regions.
  * One pointer will point to the first element outside of the contiguous `0` region (the first non-`0` element right of this region)
  * Another pointer will point to the second element outside of the contiguous `2` region (the first non-`2` element left of this region)
* If we're smart about this, we can iterate through all elements left to right until we hit the `2` region's left boundary.
  * When we see elements that belong in either region, we swap the current index with the element at that region's boundary.
  * Note: Keep in mind, when we swap with the `2` boundary, we should not increment the `curr` pointer because there is now an unseen element at the current index.


```python
class Solution:
  def sortColors(self, nums: List[int]) -> None:
    i, j = 0, len(nums) - 1
    curr = 0
    while curr <= j:
      if nums[curr] == 0:
        nums[i], nums[curr] = nums[curr], nums[i]
        i += 1
        curr += 1
      elif nums[curr] == 2:
        nums[j], nums[curr] = nums[curr], nums[j]
        j -= 1
      else: 
        curr += 1   
```

T = O(n)  
S = O(1)  

Topics = {Two Pointers, Sort}
