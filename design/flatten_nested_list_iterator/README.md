# Flatten Nested List Iterator

<b>Question:Given a nested list of integers, implement an iterator to flatten it. Each element is either an integer, or a list--whose elements may also be integers or other lists</b>
```
Input: [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,1,2,1,1].
```

Thought Process:
* Elements can either be simple integers or nested repeatedly.
* If we could perhaps have a datastructure in which the next element to be returned could somehow be placed back after being removed (in the case that we have to unwrap it)?
  * How about we use a stack? 
* We can check if the top of the stack is an integer. If so, that is the next element that should be returned on the subsequent call of `next()`.
* If the top of the stack is not an integer, we can pop the stack to unwrap the element and place all its elements back onto the stack (in reversed order).
  * Repeat as many times as necessary (for levels of nesting and elements).
* In this solution, the choice of a stack is important, but you can also use a `deque` if you prefer.

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger(object):
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

from collections import deque
class NestedIterator(object):

    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        self.stack = deque(nestedList)

    def next(self):
        """
        :rtype: int
        """
        return self.stack.popleft()
        
    def hasNext(self):
        """
        :rtype: bool
        """
        while self.stack:
          next_element = self.stack[0]
          if next_element.isInteger(): return True
          next_list = self.stack.popleft().getList()
          for n_i in reversed(next_list):
            self.stack.appendleft(n_i)

# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```

T = O(n)  
S = O(n)  

Topics = {Stack, Design}
