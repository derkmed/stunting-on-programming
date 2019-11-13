# Subarrays with K Different Integers

<b>Question:</b>

<b>Given an array A of positive integers, call a (contiguous, not necessarily distinct) subarray of A good if the number of different integers in that subarray is exactly K.</b>
<b>(For example, [1,2,3,1,2] has 3 different integers: 1, 2, and 3.)</b>
<b>Return the number of good subarrays of A.</b>

```
Example:
Input: A = [1,2,1,2,3], K = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

Thought Process:
* Let's keep two sliding windows, both right-aligned for every index.
  * One of the sliding windows will be the largest possible window ending at this index that holds `K` (unique) elements.
  * The other sliding window will be the largest subarray of the original window that holds `<K` (unique) elements.
* The two windows will always be right-aligned, but with varying left boundaries: keep in mind that the sub-window will have a higher left-index.
* Thought Process Example:

  | Data | Index | Large Window | Small Window | Corresponding Solutions | Ignored Solutions | Difference of left bounds |
  |----|----|----|----|----|----|----|
  | [<b><i>1</i></b>, 2, 1, 2, 3] | 0 | [1] | [1] | | | 1 - 1 = 0 |
  | [<b><i>1, 2</i></b>, 1, 2, 3] | 1 | [1, 2] | [2] | [1, 2,] | [1] | 1 - 0 = 0 | 
  | [<b><i>1, 2, 1</i></b>, 2, 3] | 2 | [1, 2, 1] | [1] | [1, 2, 1], [2, 1] | [1] | 2 - 0 = 2 |
  | [<b><i>1, 2, 1, 2</i></b>, 3] | 3 | [1, 2, 1, 2] | [2] | [1, 2, 1, 2], [2, 1, 2], [1, 2] | [2] | 3 - 0 = 3 |
  | [1, 2, 1, <b><i>2, 3</i></b>] | 4 | [2, 3] | [3] | [1, 2, 3] | [3] | 2 - 1 = 1 |

  * \# solutions = 7

