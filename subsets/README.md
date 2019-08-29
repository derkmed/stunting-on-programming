# Subsets

<b>
Question: Given a set of distinct integers, nums, return all possible subsets (the power set).<br><br>
Note: The solution set must not contain duplicate subsets.<br><br>
Input: nums = [1,2,3]<br>
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
<br>
</b>

Thought Process:
* You add an element or you do not
* Iterate through each element and update based on past information!

```
class Solution:
  def subsets(self, nums: List[int]) -> List[List[int]]:
    subsets = [[]]
    for num in nums:
      subsets += [subset + [num] for subset in subsets]
    return subsets
```
T = O(2<sup>n</sup>)<br>
S = O(2<sup>n</sup>)
