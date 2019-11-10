# Find First and Last Position of Element in Sorted Array

<b>Question:</b>

<b>Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.</b>   

<b>Your algorithm's runtime complexity must be in the order of O(log n).</b>   

<b>If the target is not found in the array, return [-1, -1].</b>

Thought Process:
* This definitely looks like an opportunity for binary search.
  * However, once we find an occurence of the target element, we need to keep going because we are trying to find a range, not just a single occurrenc.
* We should keep a placeholder `result` variable to keep track of the range of occurences.
  * Despite the description of what to return if the target is not found, we should start out by setting the first element of `result` to `float('inf')`.
    * Overwriting the first element of `result` is now as easy as calling `min()`.
    * Overwriting the second element of `result` is now as easy as calling `max()`.
* The general programming logic is as follows:
  * We calculate the binary search `mid`.
  * The element @ `mid` can either:
    * `== target`:
      * update result if possible.
      * recurse on the windows left and right (exclusive) of the `mid` element.
    * `< target`: overwrite the left boundary to `mid+1`
    * `> target`: overwrite the right boundary to `mid-1`
  * In all cases, we can ignore the `mid` element!
* Important: notice when bounds are updated according to `mid`, we can update them exclusively. This is also important to avoid any infinite looping.


```python
class Solution:
  def searchRange(self, nums: List[int], target: int) -> List[int]:
    result = [float('inf'), -1]
    
    def _modifiedBinarySearch(l: int, r: int):
      while l <= r:
        mid = (l + r) // 2
        if nums[mid] == target:
          result[0] = min(mid, result[0])
          result[1] = max(mid, result[1])
          if l != r:
            _modifiedBinarySearch(l, mid-1)
            _modifiedBinarySearch(mid+1, r)
          break
        elif nums[mid] < target:
          l = mid+1
        else:
          r = mid-1
      
    _modifiedBinarySearch(0, len(nums)-1)
    result[0] = -1 if result[0] == float('inf') else result[0]
    return result
```
   
T = O(logn)  
S = O(1) (O(logn) if you count stack frames from recursion)   
  
Topics = {Array, Binary Search} 
