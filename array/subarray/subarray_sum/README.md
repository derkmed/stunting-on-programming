# Subarray Sum Equals K

<b>Question: Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.<br><br>
Example:<br> 
Input: nums = [1,1,1], k = 2<br>
Output: 2<br></b>

Thought Process:
* The naive way would be to run through O(n<sup>2</sup>) possibilities. There are multiple ways to do this:
    * One way is to run `for x in range(0, n); for y in range(x + 1, n)`. We increment a running counter if `sum(nums[x:y + 1]) == k`.
    * Another way is to create `sums` in which each `index` holds the sum of all elements in `nums` up to that index, and then also run `for x in range(0, n); for y in range(x + 1, n)`. We increment a running counter if `nums[x] == k` or `nums[y] - nums[x] == k`.
 * What if we instead employ prefix sums?
     * We'll store a hash map (`counts`) to count the number of prefix sums seen (handles cases in which a longer subarray has the same prefix sum as one of its prefix subarrays. Think about this).
     * `count[V]` represents the number of previous prefix sums with value `V`
     * If our newest prefix sum has value W, and `W-V == K`, then we add `count[V]` to our answer.
     * This means that at time `t`, `A[0] + A[1] + ... + A[t-1] = W`, and there are `count[V]` indices `j` with `j < t-1` and `A[0] + A[1] + ... + A[j] = V`. Thus, there are `count[V]` subarrays `A[j+1] + A[j+2] + ... + A[t-1] = K`.
  
```python
class Solution:
  def subarraySum(self, nums: List[int], k: int) -> int:
    counts = collections.Counter()
    counts[0] = 1
    subarray_count, sum_so_far = 0, 0

    for num in nums:
      sum_so_far += num
      subarray_count += counts[sum_so_far-k]
      counts[sum_so_far] += 1
    return subarray_count
```
T=O(n)<br>
S=O(n)<br>
