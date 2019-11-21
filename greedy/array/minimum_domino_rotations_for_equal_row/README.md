# Minimum Domino Rotations For Equal Row

<b>Question:</b>

<b>In a row of dominoes, A[i] and B[i] represent the top and bottom halves of the i-th domino.  (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)</b>  
<br>
<b>We may rotate the i-th domino, so that A[i] and B[i] swap values.</b>
<br>
<b>Return the minimum number of rotations so that all the values in A are the same, or all the values in B are the same.</b>
<br>
<b>If it cannot be done, return -1.</b>


Thought Process:
* We take the greedy approach by choosing a first element number (two options) and then seeing how many instances we'd have to flip a domino.
  * We can either start with the first element in the top row (`A`) or the first element in the bottom row (`B`).
* Instead of counting flips, we can count the occurences of a desired element in either `A` or `B`. 
  * This can be done easily in an iterative fashion.
  * This effectively becomes synonymous to flipping. 
  * This is because we want to get the minimum number of matches between row `A` and row `B`. If we find the number of dominoes with the desired elements on the minority side, we should flip those as opposed to the alternative to minimize rotations.
* Special Considerations:
  * We do not have to count dominoes with the same number on both sides if they both match the starting element.
  * Invalid paths should return -1.  
  
```python
class Solution:
  def minDominoRotations(self, A: List[int], B: List[int]) -> int:
      
    def _minRotations(A: List[int], B: List[int]) -> int:
      start = A[0]
      in_a, in_b = 0, 0
      for i in range(1, len(A)):
        if A[i] != start and B[i] != start:
          return -1
        elif A[i] != start: # and B[i] == start
          in_b += 1
        elif B[i] != start: # and A[i] == start
          in_a += 1
        # else would handle the case in which the number is on both sides (we can ignore that)
      return min(in_a + 1, in_b) # whichever is the min, we must flip that side

    results = [_minRotations(A, B), _minRotations(B, A)]
    valid_results = list(filter(lambda x: x >= 0, results))
    return min(valid_results) if valid_results else -1
```

T = O(n)  
S = O(1)  

Topics = {Greedy, Array} 
