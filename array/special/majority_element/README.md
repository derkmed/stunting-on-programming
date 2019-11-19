# Majority Element

<b>Question:</b>

<b>Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.</b>

Thought Process:
* The naiive solution (Hash map with frequency counts requires O(n) space).
* Is there a way to run this algorithm in O(n) time with O(1) space? Yes!
* Boyer Moore Algorithm iterates through the array linearly, slowly pruning out prefix segments.
* At the start of each subsection, a candidate element is chosen (simply the first element).
  * We initialize a counter. 
    * From here on out, whenever we encounter an element that is equal to the candidate, we increment the counter.
    * From here on out, whenever we encounter an element that is not equal to the candidate, we decrement the counter.
  * Whenever the counter hits 0, we start a new subsection. 
* Given that it is impossible (in both cases) to discard more majority elements than minority elements, we are safe in discarding the prefix and attempting to recursively solve the majority element problem for the suffix. 
* Eventually, a suffix will be found for which count does not hit 0, and the majority element of that suffix will necessarily be the same as the majority element of the overall array.

```python
class Solution:
  def majorityElement(self, nums: List[int]) -> int:
    # Boyer Moore Algorithm
    count = 0
    candidate = None
    for num in nums:
      if count == 0: candidate = num
      count += (1 if num == candidate else -1)
    return candidate
```

T = O(n)  
S = O(1)  

Topics = {Array}
