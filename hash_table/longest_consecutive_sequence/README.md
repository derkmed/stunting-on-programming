# Longest Consecutive Sequence

<b>Question:</b>

<b>Given an unsorted array of integers, find the length of the longest consecutive elements sequence. Your algorithm should run in O(n) complexity.</b>

```
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

Thought Process:
* There's the naiive method of iterating through all elements to find the other members of a sequence up to n possible following members of a sequence for each of the n values. This takes O(n<sup>3</sup>).
* There's the other method of sorting and iterating linearly. This takes O(nlogn).
* How about we store a hash map (enabling constant time lookup) and only concern ourselves with elements that can begin a sequence (to ensure maximum sequence length).
