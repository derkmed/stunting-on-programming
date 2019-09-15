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
