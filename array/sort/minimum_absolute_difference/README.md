# Minimum Absolute Difference

<b>Question:
Given an array of distinct integers arr, find all pairs of elements with the minimum absolute difference of any two elements. 
Return a list of pairs in ascending order(with respect to pairs), each pair [a, b] follows:</b>
* <b>a, b are from arr</b>
* <b>a < b</b>
* <b>b - a equals to the minimum absolute difference of any two elements in arr</b>
 
 
```
Example: 
Input: arr = [4,2,1,3]
Output: [[1,2],[2,3],[3,4]]
Explanation: The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.
```

Thought Process:
* We can compare all n<sup>2</sup> pairings. 
* Other option, what if we only worry about the difference between two consecutive elements in a sorted version of the array?

```python
class Solution:
  def minimumAbsDifference(self, arr: List[int]) -> List[List[int]]:
    arr.sort()
    consecutive_pairs = [(x, y) for x, y in zip(arr, arr[1:])] 
    min_diff = min(y-x for x, y in consecutive_pairs)
    return [[x, y] for x, y in consecutive_pairs if y - x == min_diff]   
```

T = O(nlogn)  
S = O(n) 

Topics = {Array}
