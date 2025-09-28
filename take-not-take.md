
Many algorithmic problems—from **subsequences** to **combinatorial optimization**—rely on a simple but powerful idea: **“Take / Not Take.”** Understanding this pattern is key to solving recursion, backtracking, and dynamic programming (DP) problems efficiently.

---

## 1️⃣ What is the “Take / Not Take” Pattern?

At each step, you usually have **two choices**:

1. **Take** → Include the current element/item and update the state.
2. **Not Take** → Skip the current element/item and move to the next state.

This binary choice forms the backbone of many recursive solutions and naturally extends to DP with memoization or tabulation.

---

### General Recursion Template

```java
int helper(State state) {
    if (baseCaseReached) return baseValue;

    // Option 1: Take the current element
    int take = helper(updatedStateAfterTaking);

    // Option 2: Not take the current element
    int notTake = helper(updatedStateAfterSkipping);

    return combine(take, notTake);  // e.g., max, min, count
}
```

---

## 2️⃣ Example 1: Coin Change (Minimum Coins)

**Problem:** Given an array of coins and a target amount, find the minimum number of coins to make the amount.

**Recursive Solution (Take / Not Take):**

```java
class CoinChange {
    public int coinChange(int[] coins, int amount) {
        int ans = helper(coins, amount, coins.length - 1);
        return ans == Integer.MAX_VALUE - 1 ? -1 : ans;
    }

    private int helper(int[] coins, int left, int idx) {
        if (left == 0) return 0;                  // Exact amount formed
        if (idx < 0 || left < 0) return Integer.MAX_VALUE - 1; // Impossible

        // Not take current coin
        int notTake = helper(coins, left, idx - 1);

        // Take current coin (can reuse unlimited times)
        int take = 1 + helper(coins, left - coins[idx], idx);

        return Math.min(take, notTake);
    }
}
```

✅ **Key Idea:** At each coin, either **take it** (and stay at the same index) or **skip it** (move to previous index).

---

## 3️⃣ Example 2: Longest Increasing Subsequence (LIS)

**Problem:** Find the length of the longest strictly increasing subsequence in an array.

**Take / Not Take Recursive Solution:**

```java
class LIS {
    public int lengthOfLIS(int[] nums) {
        return helper(nums, -1, 0);
    }

    private int helper(int[] nums, int prevIdx, int curIdx) {
        if (curIdx == nums.length) return 0;

        // Not take current element
        int notTake = helper(nums, prevIdx, curIdx + 1);

        // Take current element if it is increasing
        int take = 0;
        if (prevIdx == -1 || nums[curIdx] > nums[prevIdx]) {
            take = 1 + helper(nums, curIdx, curIdx + 1);
        }

        return Math.max(take, notTake);
    }
}
```

✅ **Pattern:** Include the element in the subsequence (**take**) or skip it (**not take**).

---

## 4️⃣ Example 3: Word Break

**Problem:** Determine if a string can be segmented into words from a dictionary.

**Take / Not Take Recursive Approach:**

```java
class WordBreak {
    public boolean wordBreak(String s, List<String> words) {
        return helper(s, words, 0);
    }

    private boolean helper(String s, List<String> words, int idx) {
        if (idx == s.length()) return true;

        for (String word : words) {
            if (idx + word.length() <= s.length() && s.startsWith(word, idx)) {
                // Take current word
                if (helper(s, words, idx + word.length())) return true;
            }
        }

        // Not take any word (implicitly handled by loop)
        return false;
    }
}
```

✅ **Pattern:** Try taking each word starting at the current index. If none work, it is equivalent to **not taking** that path.

---

## 5️⃣ Why “Take / Not Take” Works

1. **Recursive decomposition:** Every problem reduces to making a choice at each step.
2. **Backtracking:** You explore all paths by taking or skipping elements.
3. **Dynamic Programming:** Memoizing or tabulating these decisions avoids recomputation.

---

## 6️⃣ Recognizing “Take / Not Take”

Ask yourself:

* Can I **choose** the current element/item?
* Can I **skip** it?
* What is the **state** after taking or skipping?
* How do I **combine** results of both choices (max, min, count, sum)?

If yes, you can likely apply this pattern.

---

## 7️⃣ Summary

* **Take / Not Take** is the backbone of many recursion, backtracking, and DP problems.
* It works for counting, maximizing, minimizing, or constructing sequences.
* Practice problems using this pattern:

  * [0/1 Knapsack](https://leetcode.com/problems/0-1-knapsack/)
  * [Coin Change](https://leetcode.com/problems/coin-change/)
  * [Word Break](https://leetcode.com/problems/word-break/)
  * [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
  * [Combination Sum](https://leetcode.com/problems/combination-sum/)

---

