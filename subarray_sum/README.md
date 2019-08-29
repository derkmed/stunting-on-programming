# Subarray Sum Equals K

<b>Question: Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.</b>

<b>Example:<br> 
Input:nums = [1,1,1], k = 2<br>
Output: 2<br>  
<b>

Thought Process:
* The naive way would be to run through O(n<sup>2</sup>) possibilities. There are multiple ways to do this:
  * One way is to run `for x in range(0, n); for y in range(x + 1, n)`. We constantly check if `sum(nums[x:y + 1]) == `k` and increment a running counter if so.
  * Another way is to create `sums` in which each `index` holds the sum of all elements in `nums` up to that index, and then also run `for x in range(0, n); for y in range(x + 1, n)`. Whenever `nums[x] == k` or `nums[y] - nums[x] == k`, we increment the running counter.
  
```
class Solution:
  def subarraySum(self, nums: List[int], k: int) -> int:
    counts = collections.Counter()
    counts[0] = 1
    subarray_count, sum_so_far = 0, 0

    for num in nums:
      sum_so_far += num
      subarray_count += counts[sum_so_far - k]
      counts[sum_so_far] += 1
    return subarray_count
```
T=O(n)<br>
S=O(n)<br>
