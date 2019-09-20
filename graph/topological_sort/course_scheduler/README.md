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
    * Use indegrees and a `visited` array. Any element that has an indegree of 0 can be added to the solution after each of its outbound neighbors gets its indegree count decremented.
  2. Use DFS
    * Use an adjacency list from courses to prereqs and prereqs to courses. This will indicate what courses to add to the processing stack first. Add thoses courses. While the stack is not empty, pop a course from the top and for each of its depending courses, remove the dependency. If any of those depending courses now has an empty list of dependencies, it is ready to be added to the stack also. After an element has been processed, you can add it to the result.
* For both methods, you must be careful about the incomplete cases (just return `[]` for solutions with incomplete lengths).

##### BFS
```python
from collections import defaultdict
class Solution:
  def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
    
    # prereqs counter (indegree)
    prereq_counts = [0] * numCourses 
    # adj mapping to dependent courses (outbound nodes)
    prereq_for = defaultdict(set)    
    
    for course, prereq in prerequisites:
      prereq_counts[course] += 1
      prereq_for[prereq].add(course)    
        
    q, solution = [c for c in range(len(prereq_counts)) if prereq_counts[c] == 0], []
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

T = O(n)  
S = O(n)  

##### DFS
```python
from collections import defaultdict
class Solution:
  def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
    
    # adj mapping to dependent courses
    prereqs_for = defaultdict(set)
    # adj mapping to prereqs
    prereqs = defaultdict(set)       
    for course, prereq in prerequisites:
      prereqs_for[prereq].add(course)  
      prereqs[course].add(prereq)
    
    # start with the elements that don't have prerequisites
    s = [c for c in range(numCourses) if not prereqs[c]]
    result = []
    while s:
      course = s.pop()
      del prereqs[course]
      for depending_course in prereqs_for[course]:
        prereqs[depending_course].remove(course)
        if not prereqs[depending_course]:
          s.append(depending_course)
      result.append(course)
    return result if not prereqs else []
  ```
  T = O(n)  
  S = O(n)  
