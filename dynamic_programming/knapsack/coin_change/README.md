# Coin Change
<B>Question: You are given coins of different denominations and a total amount of money `amount`. Write a function to compute teh fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.</b>\

```Example:
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

Thought Process:
* <b>Top-Down</b> approach:
  * Assume we know `F(S)` where some collection of change c<sub>1</sub>, c<sub>2</sub>,... for the amount `S` which is optimmal. Let's defined `C` as the "last" coin's denomination.
    * Then, the following equation could define the optimal substructure of the problem: `F(S)=F(S-C)+1`
  * Define the problem as follows:
  ```
  F(S)=F(S-C)+1 subject to S-C >= 0
  F(S=0) = 0
  ```
  * Notice that the input for `F(S)` will be bounded by `[0,amount]`. Since we can choose coins with replacement, we can memoize repeated subproblems to save time on computation.
  
```python
class Solution:
  def coinChange(self, coins: List[int], amount: int) -> int:
    dp = [0] * (amount+1)  
    def _coinChange(coins: List[int], amount: int) -> int:
      '''
      Returns the fewest number of coins needed from coins to sum up to amount.
      Returns -1 if no solution.
      '''
      nonlocal dp
      # Handle base case
      if amount == 0: return 0
      # Handle out-of-bounds case
      if amount < 0: return -1
      # Handle already seen case
      if dp[amount-1] != 0: return dp[amount-1]
      
      # Of all the non-negative options, find the minimum
      fewest_coins = float('inf')
      for coin in coins:
        prev_coin_count = _coinChange(coins, amount - coin)
        if prev_coin_count >= 0:
          fewest_coins = min(fewest_coins, prev_coin_count)
      
      dp[amount-1] = fewest_coins + 1 if fewest_coins != float('inf') else -1
      return dp[amount-1] 
      
    return _coinChange(coins, amount)
```

T = O(Sn) where S is the amount    
S = O(S) where S is the amount  

Topics = {Dynamic Programming}

Thought Process:
* <b>Bottom-Up</b> approach:
  * Similar reasoning as to <b>Top-Down</b> approach, but now in the opposite direction: we calculate `F(S)` for `S` in increasing order.
  * A `0` amount will always require 0 coins. All other amounts, we can initialize with a max-value placeholder (e.g. `float('inf')` or `amount+1`, etc.)
  * When we find another smaller solution, 
```python
class Solution:
  def coinChange(self, coins: List[int], amount: int) -> int:
    dp = [0] + [float('inf')] * (amount+1)  
    for sub_amount in range(amount+1):
      for coin in coins:
        if coin <= sub_amount:
          dp[sub_amount] = min(dp[sub_amount-coin] + 1, dp[sub_amount]) 
    return dp[amount] if dp[amount] <= amount else -1
```

T = O(Sn) where S is the amount  
S = O(S) where S is the amount  

Topics = {Dynamic Programming}
  
