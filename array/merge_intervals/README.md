# Merge Intervals

<b>Question:</b>

<b>Given a collection of intervals, merge all overlapping intervals.</b>

```
Example: 
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

Thought Process:
* Let's sort the intervals by the first element.
* We can then add the intervals to a stack, one-by-one (from left-to-right, <b>order matters</b>).
  * Whenever we encounter an interval that has a first element that is within the interval at the top of the stack, pop the stack and create a new interval.
  * Keep in mind, the intervals are already sorted by the first element.
  * Edge case: interval fits entirely within the interval prior (at the top of the stack)
* Edge case: empty list

```python
class Solution:
  def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    sorted_intervals = sorted(intervals, key=lambda l: l[0])
    solutions = []
    while sorted_intervals:
      interval = sorted_intervals.pop(0)
      if solutions and interval[0] <= solutions[-1][1]:
        last_interval = solutions.pop()
        interval[1] = max(interval[1], last_interval[1])
        interval[0] = last_interval[0]
      solutions.append(interval)

    return solutions
```

T = O(nlogn)    
S = O(n)    

## O(n) space solution:

Thought Process:
* Modify the list in place to save space!
* We can use two pointers to iterate through each element (slow and fast)
  * The slow pointer refers to the end of the window of "processed" intervals.
  * The slow pointer will refer to the last (potentially unfinished interval.
  * The fast pointer will look ahead to point to the next interval that must be added to the solution subset or merged with another interval in the solution subset.
  
```python
class Solution:
  def merge(self, intervals: List[List[int]]) -> List[List[int]]:
      intervals.sort(key=lambda l: l[0])        
      i, j = 0, 1
      while j < len(intervals):
        if intervals[i][1] >= intervals[j][0]:
          intervals[i][1] = max(intervals[j][1], intervals[i][1])
        else:
          i += 1
          intervals[i] = intervals[j]
        j += 1
      return intervals[:i+1]  
```

T = O(nlogn)  
S = O(1)  

Topics = {Array, Sort}
