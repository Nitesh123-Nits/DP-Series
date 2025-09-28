
#Take / Not Take – Part 2: Optimizing Recursion with Memoization and DP
In [Blog 1](#), we explored the **Take / Not Take** pattern in recursion, backtracking, and dynamic programming with basic examples.

In this blog, we’ll dive deeper into **how to optimize recursion using memoization** and **bottom-up dynamic programming**, turning exponential solutions into efficient ones.

---

## 1️⃣ Recap: The Take / Not Take Pattern

At each decision point:

1. **Take** → include the current element and update the state.
2. **Not Take** → skip the current element and move to the next state.

This forms the recursion tree, but **pure recursion is often exponential**.

**Example:** Coin Change without memoization has a time complexity of (O(2^n)).

---

## 2️⃣ Memoization: Avoid Recomputing States

By storing results of subproblems, we prevent recalculating them multiple times.

### Coin Change – Recursion + Memoization

```java
class CoinChangeMemo {
    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        Integer[][] memo = new Integer[n][amount + 1]; // memo[idx][leftAmount]
        int ans = helper(coins, amount, n - 1, memo);
        return ans == Integer.MAX_VALUE - 1 ? -1 : ans;
    }

    private int helper(int[] coins, int left, int idx, Integer[][] memo) {
        if (left == 0) return 0;
        if (idx < 0 || left < 0) return Integer.MAX_VALUE - 1;
        if (memo[idx][left] != null) return memo[idx][left];

        int notTake = helper(coins, left, idx - 1, memo);
        int take = 1 + helper(coins, left - coins[idx], idx, memo);

        return memo[idx][left] = Math.min(take, notTake);
    }
}
```

✅ **Key Benefit:** Reduces time complexity from **exponential** to **O(n × amount)**.

---

### 3️⃣ Bottom-Up DP: Tabulation

Instead of recursion, we can solve **Take / Not Take** problems iteratively.

#### Coin Change – Bottom-Up DP

```java
class CoinChangeDP {
    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;

        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

✅ **Pattern:** Each DP state `dp[i]` represents **minimum coins to make amount i**.
The **take / not take decision** is implicit in `Math.min(dp[i], dp[i - coin] + 1)`.

---

## 4️⃣ Another Example: Longest Increasing Subsequence (LIS)

### Memoized Top-Down

```java
class LISMemo {
    public int lengthOfLIS(int[] nums) {
        Integer[][] memo = new Integer[nums.length][nums.length + 1];
        return helper(nums, -1, 0, memo);
    }

    private int helper(int[] nums, int prevIdx, int curIdx, Integer[][] memo) {
        if (curIdx == nums.length) return 0;
        if (memo[curIdx][prevIdx + 1] != null) return memo[curIdx][prevIdx + 1];

        int notTake = helper(nums, prevIdx, curIdx + 1, memo);
        int take = 0;
        if (prevIdx == -1 || nums[curIdx] > nums[prevIdx]) {
            take = 1 + helper(nums, curIdx, curIdx + 1, memo);
        }

        return memo[curIdx][prevIdx + 1] = Math.max(take, notTake);
    }
}
```

### Bottom-Up DP

```java
class LISTabulation {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        int maxLen = 1;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }
        return maxLen;
    }
}
```

✅ **Pattern:** `dp[i]` represents **length of LIS ending at index i**. The **take / not take decision** drives the update.

---

## 5️⃣ Key Insights

1. **All “Take / Not Take” problems share a structure:**

   * Choose to include → update state.
   * Choose to skip → move to next state.

2. **Memoization:** Transform exponential recursion to polynomial time.

3. **Bottom-Up DP:** Often more efficient and avoids stack overflow for large inputs.

4. **State Design:** Identify the variables that fully describe the subproblem (e.g., index + remaining amount).

---

## 6️⃣ Common Take / Not Take Problems

| Problem                        | Link                                                                          |
| ------------------------------ | ----------------------------------------------------------------------------- |
| 0/1 Knapsack                   | [LeetCode 416](https://leetcode.com/problems/partition-equal-subset-sum/)     |
| Coin Change                    | [LeetCode 322](https://leetcode.com/problems/coin-change/)                    |
| Word Break                     | [LeetCode 139](https://leetcode.com/problems/word-break/)                     |
| Longest Increasing Subsequence | [LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/) |
| Combination Sum                | [LeetCode 39](https://leetcode.com/problems/combination-sum/)                 |

---

### 7️⃣ Summary

* **Take / Not Take** is a versatile pattern.
* **Pure recursion** is simple but slow.
* **Memoization** optimizes recursion.
* **Bottom-up DP** produces clean, efficient code.
* Recognizing the **state variables** is key to applying DP effectively.

---

