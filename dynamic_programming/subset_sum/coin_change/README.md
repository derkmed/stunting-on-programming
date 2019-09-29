# Coin Change
<B>Question: You are given coins of different denominations and a total amount of money `amount`. Write a function to compute teh fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.</b>\

```Example:
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

Thought Process:
* Top-Down approach:
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
      nonlocal dp
      if amount < 0: return -1
      if amount == 0: return 0
      if dp[amount] != 0: return dp[amount]
      
      fewest_coins = -1
      for coin in coins:
        coin_count = _coinChange(coins, amount - coin)
        if coin_count >= 0:
          if fewest_coins < 0: fewest_coins = coin_count
          else: fewest_coins = min(fewest_coins, coin_count)
            
      dp[amount] = fewest_coins + 1 if fewest_coins >= 0 else -1
      return dp[amount]
    return _coinChange(coins, amount)
```

T = O(Sn) where S is the amount    
S = O(S) where S is the amount  

Topics = {Dynamic Programming}
