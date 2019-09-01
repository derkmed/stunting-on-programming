# Word Ladder 

Thought Process:
* Run a BFS from the start word
* We know the possible word nodes outbound from any starting word will have words with only a single character (from a-z) replaced
* Because we are doing BFS, we can keep track of what words we have seen and disregard anything we come across again
* If the `end_word` is not in the `word_list` or we have no more outbound word node options, we can return 0

```
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
