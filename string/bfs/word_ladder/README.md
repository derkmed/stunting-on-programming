# Word Ladder 1

<b>Question: Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:</b>
1. <b>Only one letter can be changed at a time.</b>
2. <b>Each transformed word must exist in the word list. Note that beginWord is not a transformed word.</b>

<b>Notes:</b>
* <b>Return 0 if there is no such transformation sequence.</b>
* <b>All words have the same length.</b>
* <b>All words contain only lowercase alphabetic characters.</b>
* <b>You may assume no duplicates in the word list.</b>
* <b>You may assume beginWord and endWord are non-empty and are not the same.</b>

<b>
Input:<br>
beginWord = "hit",<br>
endWord = "cog",<br>
wordList = ["hot","dot","dog","lot","log","cog"]<br>
<br>
Output: 5<br>
Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
</b>
<br><br>

Thought Process:
* Run a BFS from the start word
* We know the possible word nodes outbound from any starting word will have words with only a single character (from a-z) replaced
* Because we are doing BFS, we can keep track of what words we have seen and disregard anything we come across again
* If the `end_word` is not in the `word_list` or we have no more outbound word node options, we can return 0

```python
class Solution:
  def ladderLength(self, begin_word: str, end_word: str, word_list: List[str]) -> int:

    standard_word_length = len(begin_word) # We can assume all words have the same length
    word_set = set(word_list)
    q = collections.deque([(begin_word, 1)])
    while q:
      word, seq_length = q.popleft()
      if word == end_word: 
        return seq_length
      for i in range(standard_word_length):
        for c in 'abcdefghijklmnopqrstuvwxyz':
          next_word = word[:i] + c + word[i+1:]
          if next_word in word_set:
            word_set.remove(next_word)
            q.append((next_word, seq_length+1))
    return 0
```

T =  O(26*len(word)*n) where n is the size of the word_list   
S =  O(n)

# Word Ladder 2
<b>Question: Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:</b>  
1. <b>Only one letter can be changed at a time  </b>
2. <b>Each transformed word must exist in the word list. Note that beginWord is not a transformed word.</b>

<b>Note:</b>  
* <b>Return an empty list if there is no such transformation sequence.</b>
* <b>All words have the same length.</b>
* <b>All words contain only lowercase alphabetic characters.</b>
* <b>You may assume no duplicates in the word list.</b>
* <b>You may assume beginWord and endWord are non-empty and are not the same.</b>
</b>

Thought Process:
* Given that we want the shortest transformation sequence(s), perhaps BFS?
* At each degree/timestep from the starting word, we definitely need to keep track of the sequence we took to get to the current position:
  * let's represent each word at layer `t` with a dictionary mapping from the word at the layer to a list of paths taken to get to this position.
  * for layer `t+1`, assuming we have not reached the end yet, we can generate a new dictionary for that layer and dispose of the previous layer.
* We can either keep a visited set <i>or</i> create a word set and remove elements as we visit them because we never want to consider a word seen at layer `u: u > t` if it was already seen at layer `t`

```python
from collections import deque
class Solution:
  def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:

    words = set(wordList)
    # mapping from  word-@-layer-t to all paths to get here
    layer = {beginWord: [[beginWord]]}
    solution = []
    while layer:
      newlayer = collections.defaultdict(list)
      for w in layer:
        if w == endWord:
          solution.extend(path for path in layer[w])
        else:
          for i in range(len(w)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
              new_word = w[:i] + c + w[i+1:]
              if new_word in words:
                newlayer[new_word] += [path + [new_word] for path in layer[w]]
      words -= set(newlayer.keys())
      layer = newlayer

    return solution

```

T =  O(26*len(word)*n) where n is the size of the word_list   
S =  O(n<sup>2</sup>)
