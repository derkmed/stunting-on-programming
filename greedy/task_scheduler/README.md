# Task Scheduler

<b>Question:</b>

<b>
Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.
</b>  
<b>
However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.
</b>  
<b>
You need to return the least number of intervals the CPU will take to finish all the given tasks.
</b>

```
Example:
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```

Thought Process:
* Being greedy is particularly helpful here because we want to get tasks with a higher count out of the way: the idea being that while these "high-count" tasks must "cool off", we can sprinkle the other tasks in during these "cool off" periods.
* We need some notion of a `run_queue` to help execute this.
  * The `run_queue` is a max-heap priority queue (negate the counts). The task with the highest number of occurences required moving forward will sit at the top.
* Furthermore, we can break the solution up into `n`-cycle length periods. Inherently, each of these `n`-cycle periods can hold at most up to one "high-count" task of each type.
  * The rest of the `n`-cycle period can be used on less frequent tasks or `idle` tasks.
  * The last period may not exactly be `n`-cycles though and can be cut off prematurely: we do not need to wait for idle tasks to complete.
  
```python
import heapq
class Solution:

  def leastInterval(self, tasks: List[str], n: int) -> int:
    task_counts = collections.Counter(tasks)
    run_queue = [(-v, k) for k, v in task_counts.items()]
    heapq.heapify(run_queue)
    
    cycles = 0
    while run_queue:
      
      i, wait_queue = 0, []
      while i <= n:
        if run_queue:
          negated_count, task = heapq.heappop(run_queue)
          if negated_count + 1 != 0: wait_queue.append((negated_count + 1, task))
        else:
          if wait_queue: i = n+1
          break
        i += 1
      
      for (negated_count, task) in wait_queue:
        heapq.heappush(run_queue, (negated_count, task))
      cycles += i
      
    return cycles                  
```
T = O(nlogn)  
S = O(n)  
  
Advanced Thought Process:
* We can do this purely from an analytical standpoint.
* e.g. Take the tasks `['A', 'A', 'A', 'A', 'B', 'B', 'B', 'B', 'C', 'D']` with a cycle cooldown of 2.
* The solution might look something like: `['A', 'B', 'C', 'A', 'B', 'D', 'A', 'B', 'idle', 'A', 'B']`
* But another way to view this is as follows:
  
   | offset | 0 | 1 | 2 |
   | --- | --- | --- | --- |
   | 0 | A | B | C |
   | 3 | A | B | D |
   | 6 | A | B | idle |
   | 9 | A | B |  
   
* If we observe closely, we can see there's something particularly mathematical in this approach.
  * The solution <i>appears</i> that it will always be `(maximum_task_count - 1) * (n + 1) + tasks_with_max_count`
  * Edge case is to lower bound the above value by the # of all tasks (consider the case in which `n=0`).

```python
class Solution:
  def leastInterval(self, tasks: List[str], n: int) -> int:
    task_freqs = collections.Counter(tasks)
    max_freq = max(task_freqs.values())
    max_freq_tasks = sum(task_freqs[i] == max_freq for i in task_freqs.keys())
    return max((max_freq - 1) * (n + 1) + max_freq_tasks, len(tasks))
```

T = O(n)  
S = O(1)  

Topics = {Greedy, Priority Queue, Queue, Array}
