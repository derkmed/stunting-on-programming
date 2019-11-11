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

Topics = {Greedy, Priority Queue, Queue, Array}
