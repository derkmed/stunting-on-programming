# Product of Array Except Self

<b>Question:</b>  
<b>Given an array `nums` of `n` integers where `n>1`, return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.<b/>
  
<b>Note: You must solve this without division and in O(n)</b>  
<b>Note: Try to solve this in constant space complexity (not including the output array)</b>  

```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

Thought Process:
* Notice that each index of the output should hold the combined product of two smaller products:
  1. the products of all the elements left of this index in the corresponding input
  2. the products of all the elements right of this index in the corresponding input
* What if we run 2 passes:
  1. (left to right) the first to assign each output array element to the products of all elements left of its index in the corresponding input array 
  2. (right to left) the second to scale the output array element at each index by the products of all elements right of its index in the corresponding input array  

```python
class Solution:
  def productExceptSelf(self, nums: List[int]) -> List[int]:
    output = [1 for _ in range(len(nums))]
    l_product, r_product = 1, 1
    for i in range(len(nums)):
      output[i] = l_product
      l_product *= nums[i]
    for i in reversed(range(len(nums))):
      output[i] *= r_product
      r_product *= nums[i]
    return output
```

T = O(n)  
S = O(n)  (O(1) if you don't count the output array)  

Topics = {Array, Two Pass}

  

