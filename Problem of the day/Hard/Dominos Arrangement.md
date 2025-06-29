# Intuition:

- You are tiling a `3 x N` board using `2 x 1` dominoes.
- You **cannot** tile a `3 x N` board **if `N` is odd**.
- If `N` is **even**, then the solution exists and follows a **recurrence relation**:
  
  Let `dp[n]` represent the number of ways to tile a `3 x n` board:

  ```
  dp[0] = 1 (Empty board)
  dp[2] = 3
  dp[4] = 11
  dp[6] = 41
  dp[8] = 153
  ...
  ```

---

# Approach:

```
dp[n] = 4 * dp[n - 2] - dp[n - 4]
```

This recurrence is valid **only for even n** (since odd lengths can't be fully tiled with 2x1 dominoes on a 3-row board).

---
# Complexity Analysis:

- **Time:** `O(n/2)` → because we compute only for even `i`.
- **Space:** `O(n)` → can be optimized to `O(1)` using rolling variables.

---

# Code:

```java
public class Solution {
    private static final int MOD = 1000000007;

    public static int dominosRearrangement(int n) {
        if (n % 2 != 0) return 0; // Impossible for odd N

        int[] dp = new int[n + 1];
        dp[0] = 1;
        if (n >= 2) dp[2] = 3;

        for (int i = 4; i <= n; i += 2) {
            dp[i] = (int)(((4L * dp[i - 2]) % MOD - dp[i - 4] + MOD) % MOD);
        }

        return dp[n];
    }
}
```

---

### **Example Execution:**

For input `N = 2`,  
`dp[2] = 3` (3 ways to tile 3x2)  
For input `N = 3`,  
`return 0` (odd dimension — not tileable)  
For input `N = 4`,  
`dp[4] = 11`

---

