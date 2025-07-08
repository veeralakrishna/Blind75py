
# 17. Coin Change

## Problem Statement

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = [2], amount = 3
**Output:** -1

**Example 3:**

**Input:** coins = [1], amount = 0
**Output:** 0

## Solution

```python
class Solution:
    def coinChange(self, coins: list[int], amount: int) -> int:
        # Create a DP array to store the minimum coins for each amount
        # Initialize with a value larger than any possible number of coins
        dp = [float('inf')] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0:
                    dp[i] = min(dp[i], dp[i - coin] + 1)

        return dp[amount] if dp[amount] != float('inf') else -1
```

## Explanation

This is a classic dynamic programming problem.

We create a `dp` array where `dp[i]` will store the minimum number of coins required to make the amount `i`.

1.  **Initialization:** We initialize the `dp` array with a large value (infinity) to represent that the amounts are not yet reachable. `dp[0]` is set to 0 because it takes 0 coins to make an amount of 0.

2.  **Bottom-Up Calculation:** We iterate from amount 1 up to the target `amount`. For each amount `i`, we iterate through our available `coins`.

3.  **DP Relation:** If a `coin` is less than or equal to the current amount `i`, we can potentially use this coin. The number of coins to make `i` would be `dp[i - coin] + 1`. We take the minimum of this value and the current `dp[i]`.
    -   `dp[i] = min(dp[i], dp[i - coin] + 1)`

4.  **Final Result:** After filling the `dp` array, `dp[amount]` will hold the minimum number of coins for the target amount. If it's still infinity, it means the amount is unreachable, so we return -1.

The time complexity is O(amount * n), where n is the number of coins, because of the nested loops. The space complexity is O(amount) for the `dp` array.
