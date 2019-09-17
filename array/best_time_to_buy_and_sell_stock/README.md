# Best Time to Buy and Sell Stock

<b>
Question: Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit. Note that you cannot sell a stock before you buy one. </b>
```
Example:  
Input: [7,1,5,3,6,4]  
Output: 5  
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.  Not 7-1 = 6, as selling price needs to be larger than buying price.
```


Thought Process:
* Linearly walk through the array. Always keep track of the smallest and largest element seen to calculate the current profit scheme. 
* Whenever we see a smaller element, reset the hi pointer.
  * This works because we should already have the last observed profit recorded, if it was the highest. 
  * Since we have just passed a new smallest element, assuming the largest element for the maximum profit is later on in the array, we can discard the previous largest/smallest-element pair.

```python
class Solution:
  def maxProfit(self, prices: List[int]) -> int:
      max_profit = 0
      hi, lo = -float("inf"), float("inf")
      for price in prices:
        if price < lo:
          lo = price
          hi = -float("inf")
        elif price > hi:
          hi = price
        max_profit = max(max_profit, hi - lo)
      return max_profit
```
T = O(n)  
S = O(1)  
