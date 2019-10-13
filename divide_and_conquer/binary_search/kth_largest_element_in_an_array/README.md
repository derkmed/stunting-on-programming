# Kth Largest Element in an Array

<b>Question: Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.</b>

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```


Thought Process:
* We can sort the array in ascending order and then return the kth element from the right. This will require O(nlogn) time.
* How about we try to optimize the average-case runtime complexity, instead?
  * Let's employ a partitioning approach in which given a pivot element in a window of the array, we place all elements:
    * `< `the pivot element to the left 
    * all elements `>=` the pivot element to the right.
  * Note: <i>for the purposes of this implementation</i>, it's important that we apply the `>=` to the right and '<' as opposed to `<=` on the left and `>` on the right for the cases in which duplicate elements may appear.
  * At the end of the partitioning, we can return the index for which the pivot element belongs in the sorted array.
   * We can repeat this process on smaller windows left or right of the pivot index, depending on its relative magnitude in compariwon to `n-k`.
   * To mitigate the possibility of getting hit with the worst case scenario (an array always getting sorted in descending order), we can shuffle the input array randomly prior to processing.

```python
from typing import Tuple
class Solution:
  
  def partition(self, nums: List[int], i: int, j: int) -> Tuple[List[int], int]: 
    '''
    This will choose the first element in the [i, j) window to be a pivot. It will return the index
    that this pivot belongs in the sorted array.
    '''
    
    def swap(nums: List[int], i: int, j: int):
      nums[i], nums[j] = nums[j], nums[i]
      
    pivot, pivot_index, i = nums[i], i, i+1
    while i <= j:
      if nums[i] < pivot: i += 1
      else:
        swap(nums, i, j)
        j -= 1
    swap(nums, pivot_index, j)
    return (nums, j)
  
  def findKthLargest(self, nums: List[int], k: int) -> int:
    random.shuffle(nums)
    
    result_index = 0    
    lo, hi, k_complement = 0, len(nums) - 1, len(nums) - k
    while result_index <= k_complement:
      
      nums, pivot_index = self.partition(nums, lo, hi)      
      result_index = pivot_index
      if pivot_index < k_complement:
        lo = pivot_index + 1
      elif pivot_index > k_complement:
        result_index -= pivot_index
        hi = pivot_index
      else:
        break
    return nums[result_index]
       
```

T = O(n<sup>2</sup>) (However, this can be T(n), which is why it may be preferred over sorting)
S = O(1)
