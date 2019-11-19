# Container with Most Water

<b>Question: Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.  
Note: You may not slant the container and n is at least 2.
  
Example:  
Input: [1,8,6,2,5,4,8,3,7]  
Output: 49  
</b>

Thought Process: 
* Obviously there is the `(n * (n-1)) / 2` solution with constant space. Can we do better?
* Let's keep a pair of two pointers `i` and `j`. Observe that the container area is dictated by the minimum of the two heights of the left and right container boundaries.
* We can greedily take the smaller pointer and move it inwards (`i++` or `j--`) and recalculate the container area to see if we found a new maximum.
  * This is okay because the area will either consistently decrease or increase. Assuming it only decreases, well we already found the solution. Assuming it will eventually increase by moving our pointers inwards, we will eventually come to the solution.


```python
class Solution:
  def maxArea(self, heights: List[int]) -> int:
      i, j = 0, len(heights) - 1
      solution = min(heights[i], heights[j]) * ( j - i )
      while (i < j):
        if (heights[i] < heights[j]):
          i += 1 
          solution = max(solution, min(heights[i], heights[j]) * ( j - i ))
        else:
          j -= 1 
          solution = max(solution, min(heights[i], heights[j]) * ( j - i ))
      return solution
```
T = O(n)  
S = O(1)  

Topics: {Array, Two Pointers}  
