# Median of Two Sorted Arrays

<b>Question: There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)). You may assume nums1 and nums2 cannot be both empty.</b>

Thought Proces:
* We define two pointers `i` and `j`that divide the array in half as follows:  
  * ```
          left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
    ```
  * `i = 0 ~ m` and `j = (m + n + 1) / 2 - i` because the are `m + n + 1` places a cut can be placed (think about this:if A and B should be arranged contiguously, between them is a space!)
  * We want to ensure that both the `left_part` and `right_part` are equal in length and that the `max(left_part) <= min(right_part)`. Thus, move the "cut" making sure that B[j-1] <= A[i]` and `A[i-1] <= B[j]`.
  * Based on this definition, we need to make sure that n is the longer length to ensure that `j >= 0`.
* Depending on the length of the array, the goal we are trying to go for is to be able to calculate the median such that `median = (max(left_part) + min(right_part))/2`.
* We need proper bounds checking and to handle when we reach either end of the arrays.

Working example:
``` 
A = [1, 5, 7, 9]     
B = [0, 2, 4]  
```

``` 
A = [0, 2, 4]  
B = [1, 5, 7, 9]     
```

``` 
A:     0     |  2  4  
B:  1  5  7  |  9  
  
i = 1 [i_min = 0, i_max = 3]  
j = 3 = (3 + 4 + 1) / 2 - 1  
but notice that  A[i] < B[j-1]
```

``` 
A:  0  2  | 4  
B:  1  5  | 7  9

i = 2 [i_min = 2, i_max = 3] 
j = 2 = (3 + 4 + 1) / 2 - 2
but notice that  A[i] < B[j-1]  
```

``` 
A:  0  2  4  |
B:     1     | 5    7  9

i = 3 [i_min = 3, i_max = 3] 
j = 3 = (3 + 4 + 1) / 2 - 3
solution is 4!
```


```python
class Solution:
  def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
    '''
    i + j = m - i + n - j (+1); i =0 ~ m and j = (m + n + 1) / 2 - i
    NOTE: we need to bound n >= m because of our j equation.
      
      nums1[i-1] <= nums2[j]
      nums2[j-1] <= nums1[i]
      
    median = (max_left + min_right) / 2
    
    i + j = m + n + 1 - i - j
    j = (m + n + 1) / 2 - i ==> make sure that n >= m
    '''
    m, n = len(nums1), len(nums2)
    if m > n:
      nums1, nums2 = nums2, nums1
      m, n = n, m
    
    i, j = 0, (m + n - 1) // 2
    i_min, i_max = 0, m
    while i_min <= i_max:
      i = (i_min + i_max) // 2
      j = (m + n + 1) // 2 - i  
      
      if i < m and nums1[i] < nums2[j-1]: 
        # We check on i because j is pretty much guaranteed to be in bounds. Think about why
        i_min = i + 1
      elif i > 0 and nums2[j] < nums1[i-1]:
        i_max = i - 1
      else:
        
        left_max = -float('inf')
        if i > 0: left_max = max(left_max, nums1[i-1])
        if j > 0: left_max = max(left_max, nums2[j-1])    
        
        if (m + n) % 2 != 0: return float(left_max)
        
        right_min = float('inf')
        if i < m: right_min = min(right_min, nums1[i])
        if j < n: right_min = min(right_min, nums2[j])
          
        return (left_max + right_min) / 2
  ```


T = O(log(mn))   
S = O(1)
