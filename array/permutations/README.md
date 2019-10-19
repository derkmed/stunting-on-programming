# Next Permutation  

<b>Question:</b> 
<b>Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers. If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order). The replacement must be in-place and use only constant extra memory. Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.</b>

Thought Process:
* When going through permutations, think about how they are made.
  * We go through each index `i` and select some value `v`. We can treat this temporarily as fixed in a sense.
  * For all the indices after this "fixed" index, the selection space is constrained to no longer include `v`.
  * Once all the permutations from `i+1` onwards have been selected however, we now need to backtrack and allow the `v`@`i` to be swapped with another element in the selection space for `i` onwards.
* We can split the current permutation into the two following components:
  * The "fixed" segment of the permutation.
  * A "decreasing" segment of the permutation: this is the segment that we are free to permutate.
    * All numbers in this segment are strictly decreasing or equal (for duplicates).
    * Once we've exhausted the possible permutations for this segment, we will have to include the last index of the "fixed" segment (the one before this segment) in our permutation space: the cardinality of the permutation space has increased by 1 element.
    * We must swap the new element with the smallest element in the "decreasing" segment larger than itself.
    * We can then reverse the modified "decreasing" segment.
* Edge Case Considerations:
  * Make sure to handle duplicate digits/numbers.

Example:
* `[1 2 5 6 4 3]` can be broken up into `[1 2 5]` ("fixed-so-far") and `[6 4 3]` ("decreasing").
* We know that there's no number starting with `[1 2 5]` that is larger than the current `[1 2 5 6 4 3]`
  * We'll have to expand our permutation search space to include the `5`
  * Let's swap it with the smallest element larger than itself (`6`).
  * We now have `[1 2 6]` and `[5 4 3]`
  * We can simply reverse the "decreasing" segment and add it back to the list to get the next permutation in ascending order (`[1 2 6 3 4 5]`).


```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
    
        if not nums: return
        
        # First find the starting index for the decreasing segment
        n = len(nums)
        i = n-2
        while i >= 0 and nums[i] >= nums[i+1]: i -= 1
        decrease_index = i+1
        
        # Edge case for largest permutation
        if decrease_index <= 0:
            nums = nums.reverse()
            return
            
        # Now find the index to swap with the index prior to the decreasing segment
        j = n-1
        while j >= decrease_index and nums[j] <= nums[decrease_index-1]: j -= 1
        nums[decrease_index-1], nums[j] = nums[j] , nums[decrease_index-1]
        
        # After the swap, we can simply reverse the modified decreasing segment
        nums[decrease_index:] = reversed(nums[decrease_index:])
```

T = O(n)  
S = O(1)   
Topics = {Array}
