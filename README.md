# Return-the-maximum-dot-product-between-non-empty-subsequences

This is **LeetCode 1458 – Max Dot Product of Two Subsequences**.
It’s a **classic Dynamic Programming (DP)** problem with a tricky edge case (all products negative).

---

## 🔍 Key Idea

We must:

* Choose **non-empty subsequences**
* From **both arrays**
* Of the **same length**
* Maximize their **dot product**

Because order must be preserved → **DP on indices**.

---

## 🧠 Why Greedy Fails?

You can’t just pick largest values:

* Sometimes skipping elements gives better alignment
* Negative numbers can flip the result
* We must ensure **at least one pair is chosen**

So we use **DP with forced selection**.

---

## 🧩 DP Definition

Let:

```
dp[i][j] = maximum dot product using
           subsequences from nums1[0..i-1] and nums2[0..j-1]
           with at least one pair selected
```

### Transition

At `(i, j)` we have 3 choices:

1. **Pair nums1[i-1] with nums2[j-1]**

```
nums1[i-1] * nums2[j-1] 
+ max(0, dp[i-1][j-1])
```

2. **Skip nums1[i-1]**

```
dp[i-1][j]
```

3. **Skip nums2[j-1]**

```
dp[i][j-1]
```

So:

```
dp[i][j] = max(
    nums1[i-1] * nums2[j-1] + max(0, dp[i-1][j-1]),
    dp[i-1][j],
    dp[i][j-1]
)
```

---

## ⚠️ Important Edge Case

If **all products are negative**, the formula above might return `0` (invalid because subsequence must be non-empty).

So we initialize DP with **very small values** (`-1e9`) to **force picking at least one pair**.

---

## ✅ Java Solution (Optimized & Accepted)

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int m = nums2.length;

        int[][] dp = new int[n + 1][m + 1];
        int NEG_INF = -1000000000;

        // Initialize with negative infinity
        for (int i = 0; i <= n; i++) {
            for (int j = 0; j <= m; j++) {
                dp[i][j] = NEG_INF;
            }
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                int product = nums1[i - 1] * nums2[j - 1];

                dp[i][j] = Math.max(
                        product,
                        product + Math.max(0, dp[i - 1][j - 1])
                );

                dp[i][j] = Math.max(dp[i][j], dp[i - 1][j]);
                dp[i][j] = Math.max(dp[i][j], dp[i][j - 1]);
            }
        }

        return dp[n][m];
    }
}
```

---

## ⏱️ Complexity

* **Time:** `O(n * m)`
* **Space:** `O(n * m)`
* (Can be optimized to `O(m)` with rolling array)

---

## 📌 Example Walkthrough (Example 3)

```
nums1 = [-1, -1]
nums2 = [1, 1]
```

Best possible:

```
-1 * 1 = -1
```

DP correctly returns **-1**, not 0 ✔️

---

## 🧠 Interview Tip

If asked:

> “Why not initialize dp with 0?”

Answer:

> Because subsequences must be **non-empty**, and initializing with 0 would incorrectly allow choosing nothing.


