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
    '''
    
    m, n = len(nums1), len(nums2)
    if m > n:
      nums1, nums2, m ,n = nums2, nums1, n, m
    if n == 0:
      raise ValueError
    
    i_min, i_max = 0, m
    while i_min <= i_max:
      i = (i_min + i_max) // 2
      j = (m + n + 1) // 2 - i
      if i < m and nums1[i] < nums2[j-1]:
        i_min = i + 1 # increase i: it is too small
      elif i > 0 and j < n and nums2[j] < nums1[i-1]:
        i_max = i - 1 # decrease i: it is too large
      else:
        max_of_left = -1
        # these first two cases handle the out of bounds scenario!
        if i == 0:
          max_of_left = nums2[j-1]
        elif j == 0:
          max_of_left = nums1[i-1]
        else:
          max_of_left = max(nums1[i-1], nums2[j-1])
        
        # odd length case
        if (m + n) % 2 == 1:
          return max_of_left
        
        min_of_right = -1
        # these first two cases handle the out of bounds scenario!
        if i == m:
          min_of_right = nums2[j]
        elif j == n:
          min_of_right = nums1[i]
        else:
          min_of_right = min(nums1[i], nums2[j])

        return (max_of_left + min_of_right) / 2.0
  ```


T = O(log(mn))   
S = O(1)