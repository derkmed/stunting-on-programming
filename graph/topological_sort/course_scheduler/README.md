# Course Scheduler II

<b>
Question: There are a total of n courses you have to take, labeled from 0 to n-1. Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]  
Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.
There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.
</b>
```
Example:  
Input: 2, [[1,0]]   
Output: [0,1]  
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].  
```

Thought Process:
* This is classic topological sort
* There are two ways to solve topological sort
  1. Use BFS
  2. Use DFS

<b>BFS:</b>
```python
from collections import defaultdict
class Solution:
  def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
    
    # indegree counter
    prereq_counts = [0] * numCourses 
    # adj mapping to dependent courses
    prereq_for = defaultdict(set)
    q, solution = [], []
    
    for course, prereq in prerequisites:
      prereq_counts[course] += 1
      prereq_for[prereq].add(course)
    
    for prereq_i, count in enumerate(prereq_counts):
      if  count == 0:
        q.append(prereq_i)

    while q:
      course = q.pop()
      for dependent_course in prereq_for[course]:
        prereq_counts[dependent_course] -= 1
        if prereq_counts[dependent_course] == 0:
          q.append(dependent_course)
              
      del prereq_for[course]
      solution.append(course)
      
    return solution if len(solution) == numCourses else []
       
```
