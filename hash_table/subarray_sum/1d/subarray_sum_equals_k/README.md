# Subbary Sum Equals K

<b>Question: Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k. </b>

Thought Process
* The naive solution would be to iterate through all possible subarray bounds in O(N^2)
* A better solution would be to recognize that if we kept prefix sums (sums from the beginning of the array through an index:
** two prefix sums `sums[i]` and `sums[j]` being equivalent implies that the subarray between them has a sum of 0
** two prefix sums `sums[i]` and `sums[j]` having a difference of k implies that the subarray between them has a sum of `k`

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        sums = defaultdict(int)
        sums[0] = 1
        cumSum, result = 0, 0
        for num in nums:           
            cumSum += num
            result += sums.get(cumSum - k, 0)  # cumSum - prevCumSum = k --> cumSum - k = prevCumSum
            sums[cumSum] += 1
        return result

```


T = O(N)  
S = O(N)
