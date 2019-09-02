# 3Sum
<b>Question: Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero. The solution set must not contain duplicate triplets.</b>

Thought Process:
* There are nC3 combinations. That leads to approx O(n<sup>3</sup>) iterations.
  * If we are smart however, maybe we can bring this down to O(n<sup>2</sup>)?
* What if we first sort the list?
  * We can iterate through each element `i` in `[0, n - 2)` and for each element, we can have a shrinking window from `[i + 1, n - 1]`
* We need to consider at what point can we immediately break out? 
  * When the smallest number is larger than 0, there's no point continuing
* We need to consider possible edge cases
  * There may be duplicate elements, and we do not want to return duplicate triplets.

```
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
      triplets = []
      nums, n = sorted(nums), len(nums)
      for i in range(n - 2):
        if (nums[i] > 0): break
        if (i > 0 and nums[i] == nums[i - 1]): continue
          
        l, r = i + 1, n - 1
        while l < r:
          curr_sum = nums[i] + nums[l] + nums[r]
          if (curr_sum < 0):
            l += 1
          elif (curr_sum > 0):
            r -= 1
          else:
            triplets.append([nums[i], nums[l], nums[r]])            
            while l < r and nums[l] == nums[l + 1]:
              l += 1
            while r > l and nums[r] == nums[r - 1]:
              r -= 1
            l += 1
            r -= 1
            
      return triplets       
```

T=O(n<sup>2</sup>)<br>
S=O(n<sup>2</sup>)

# 3Sum Closest

<b>Question: Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.</b>

Thought Process:
* Similar to regular 3Sum, but now we want the value closest to `target` as opposed to `0`
* Start with the initial default solution (`nums[0] + nums[1] + nums[2]`) as a placeholder and then run the regular 3Sum logic from there.

```
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums, n = sorted(nums), len(nums)
        solution = nums[0] + nums[1] + nums[2]
        solution_distance = target - solution
        
        for i in range(0, n - 2):
          l, r = i + 1, n - 1   
          while l < r:
            curr_sum = nums[i] + nums[l] + nums[r]
            candidate_dist = abs(curr_sum - target)
            if (candidate_dist < solution_distance): 
                solution, solution_distance = curr_sum, candidate_dist
            if (curr_sum < target):
              l += 1
            elif (curr_sum > target):
              r -= 1
            else:
              return curr_sum
              
        return solution
```

T=O(n<sup>2</sup>)<br>
S=O(1)


# Valid Triangle Number

<b>Question: Given an array consisting of non-negative integers, your task is to count the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.</b>

Thought Process:
* There are nC3 combinations. That leads to approx O(n<sup>3</sup>) iterations.
  * If we are smart however, maybe we can bring this down to O(n<sup>2</sup>)?
* A valid triangle consists of 3 sides `a`, `b`, `c` such that `a + b > c`, for all possible assignments of `a`, `b`, `c` to each side. 
* This is very similar to 3Sum. 
  * How about, we sort in descending order. This way, if we move from left-to-right, we know  the maximum size `c` that `a + b` must be greater than.
  
```
class Solution:
  def triangleNumber(self, nums: List[int]) -> int:
    nums, count_result, n = sorted(nums, reverse=True), 0, len(nums)
    for i in range(n):
      j, k = i + 1, n - 1
      while j < k:
        if nums[j] + nums[k] > nums[i]:
          count_result += k - j
          j += 1
        else: 
          k -= 1
    return count_result
```
T=O(n<sup>2</sup>)<br>
S=O(1)

# 3Sum Multi

<b>Question: Given an integer array A, and an integer target, return the number of tuples i, j, k  such that i < j < k and A[i] + A[j] + A[k] == target. As the answer can be very large, return it modulo 10^9 + 7.</b>

Thought Process:
* The whole `i < j < k` just throws you off. It's confusing. They essentially just want the tuples.
* Similar to 3Sum, however now we must take into account there can be duplicate elements
* This will require combinatorics
* Let's redefine `i`, `j`, and `k` as the actual elements of the integer array. There are 3 cases among the possible combinations:
  1. `i == j == k`
  2. `i == j != k`
  3. `i < k && j < k && i != j`
   * Why it is required to check (i < j && j < k) in case 3:
    * Suppose the input is {1,2,3} and target is 6. So we are going to have the following combination:
     1. {1,2,3} {i=1,j=2,k=3}
     2. {1,3,2} {i=1,j=3,k=2}
     3. {2,1,3} {i=2,j=1,k=3}
     4. {2,3,1} {i=2,j=3,k=1}
     5. {3,1,2} {i=3,j=1,k=2}
     6. {3,2,1} {i=3,j=2,k=1}
      the input must be one and only one of these combinations. Ttherefore, we just need to pick the relation which occurs exactly only once as the condition. 
       * You can use 1.(i<j && j<k) because the relation occurs only one.
       * You can use 6.(i>j && j>k) which also occurs once.
       * But you cannot use 3.5.(i>j && j<k) because this relation occurs twice, nor can you use 2.4.(i<j && j>k) because it occurs twice too.

```
class Solution:
  def threeSumMulti(self, nums: List[int], target: int) -> int:
    nums_counts = collections.Counter(nums)
    res = 0
    for i, j in itertools.combinations_with_replacement(nums_counts, 2):
      k = target - i - j
      if i == j == k: 
        # nchoosek(nums_counts[i], 3)
        res += nums_counts[i] * (nums_counts[i] - 1) * (nums_counts[i] - 2) / 6
      elif i == j != k: 
        # nchoosek(nums_counts[i], 2) * nums_counts[k]
        res += nums_counts[i] * (nums_counts[i] - 1) / 2 * nums_counts[k]
      elif k > i and k > j:
        # implied that i != j
        res += nums_counts[i] * nums_counts[j] * nums_counts[k]
    return int(res % (10**9 + 7))
```

T = O(N<sup>2</sup>)  
S = O(N)
