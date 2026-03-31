# Intuition

Number of unique BSTs formed using `1..N` is given by **Catalan Numbers**:

$$C(n) = Σ (C(i-1) * C(n-i))  for i = 1 → n
$$
This represents choosing each node as root:

* left subtree → `(i-1)` nodes
* right subtree → `(n-i)` nodes

---

# Approach

Use DP:

1. `dp[0] = 1`, `dp[1] = 1`
2. For each `n`:

   * compute using Catalan recurrence
3. Return `dp[num] % MOD`

---

# Complexity

* **Time complexity:**
$$O(N^2)
$$
* **Space complexity:**
$$O(N)
$$
---

# Code

```java
public class Solution {

    static final int MOD = 1000000007;

    public static long totalTrees(int num) {

        long[] dp = new long[num + 1];

        dp[0] = 1;
        if (num >= 1) dp[1] = 1;

        for (int n = 2; n <= num; n++) {

            long ways = 0;

            for (int i = 1; i <= n; i++) {
                ways = (ways + (dp[i - 1] * dp[n - i]) % MOD) % MOD;
            }

            dp[n] = ways;
        }

        return dp[num];
    }
};

```

---

# Example walkthrough

For `N = 3`:

```
dp[0] = 1
dp[1] = 1

dp[2] = dp[0]*dp[1] + dp[1]*dp[0] = 2
dp[3] = dp[0]*dp[2] + dp[1]*dp[1] + dp[2]*dp[0] = 5
```

---

# Key Insight

Unique BST count = Catalan Number
