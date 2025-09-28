
# How to Decide the Size and Dimensions of Memoization or DP Arrays

Dynamic programming (DP) and memoization are powerful techniques for solving recursion-based problems efficiently. A common challenge is **deciding the size and dimensions of your cache or DP array**. This decision is crucial because it ensures **all subproblems are stored without wasting space** or overwriting results.

---

## 1️⃣ Understanding DP State

Every DP or memoization array stores solutions to **subproblems**. To determine its size:

1. Identify the **state variables**—the parameters that uniquely define a subproblem.
2. Decide the **range of values** each state variable can take.
3. Map each state variable to a dimension in the array.

**Rule of Thumb:**

> One state variable → one dimension in the DP array.

---

## 2️⃣ Step-by-Step Approach

1. **List all recursive parameters** that influence the result.
2. **Determine the valid range** of each parameter.

   * For array indices, size = `maxIndex + 1`.
   * For amounts or sums, size = `maxValue + 1`.
3. **Decide the array type**:

   * `Boolean[]` for true/false decisions.
   * `Integer[]` or `int[]` for counts, minimums, or maximums.
4. **Handle negative values**:

   * Use an offset to convert negative indices to positive.
5. **Allocate a multi-dimensional array** if there is more than one state variable.

---

## 3️⃣ Common Examples

### Example 1: Coin Change (Take / Not Take)

**Recursive function:**

```java
helper(coins, remainingAmount, coinIndex)
```

* **State variables:** `coinIndex`, `remainingAmount`
* **Range:** `coinIndex = 0..n-1`, `remainingAmount = 0..amount`
* **Memoization array:**

```java
Integer[n][amount + 1];
```

---

### Example 2: Longest Increasing Subsequence (LIS)

**Recursive function:**

```java
helper(nums, prevIndex, currentIndex)
```

* **State variables:** `currentIndex`, `prevIndex`
* **Range:** `currentIndex = 0..n-1`, `prevIndex = -1..n-1`
* **Offset negative index:** store `prevIndex + 1`
* **Memoization array:**

```java
Integer[n][n + 1]; // prevIndex + 1
```

---

### Example 3: Word Break

**Recursive function:**

```java
helper(s, currentIndex)
```

* **State variable:** `currentIndex`
* **Range:** `0..s.length()`
* **Memoization array:**

```java
Boolean[s.length()];
```

---

## 4️⃣ Key Guidelines

| Principle                      | Explanation                                               |
| ------------------------------ | --------------------------------------------------------- |
| Include all state variables    | Missing any variable can lead to incorrect results.       |
| Dimension size = max value + 1 | Ensure the entire range fits in the array.                |
| Offset negative indices        | Convert them to positive to use as array indices.         |
| Choose correct array type      | Boolean, Integer, int, etc., depending on what you store. |

---

## 5️⃣ Pitfalls to Avoid

* Using a 1D array when recursion depends on two parameters → results will be incorrect.
* Incorrect array size → index out-of-bounds errors.
* Ignoring negative indices → array index must always be non-negative.

---

## 6️⃣ Summary

Designing DP/memoization arrays boils down to **capturing all subproblem states**:

1. Identify all parameters that define a subproblem.
2. Determine their ranges and offsets if needed.
3. Allocate an array with the appropriate dimensions.
4. Use proper types for values being stored.

Following these steps ensures your recursive or DP solution is **correct, efficient, and avoids redundant calculations**.
