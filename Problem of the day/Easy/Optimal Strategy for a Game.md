# Intuition

This is a classic two-player optimal play problem. Every time you pick a coin (left or right), your opponent will also pick optimally to reduce your final gain. So at every decision, you must assume your opponent will force the worst possible outcome for you. Dynamic programming models this interaction.

---

# Approach

Define `dp[i][j]` as the maximum amount you can win if the subarray from index `i` to `j` is still available.

When you choose a value, your opponent will also choose optimally. So:

If you pick the left coin (`coins[i]`), the opponent can:
* Pick from `(i+1, j)` leaving you `(i+2, j)` or `(i+1, j-1)`.

If you pick the right coin (`coins[j]`), the opponent can:
* Pick from `(i, j-1)` leaving you `(i+1, j-1)` or `(i, j-2)`.

So:

```
dp[i][j] = max(
    coins[i] + min(dp[i+2][j], dp[i+1][j-1]),
    coins[j] + min(dp[i+1][j-1], dp[i][j-2])
)
```

We compute this bottom-up using increasing lengths.

Final result is `dp[0][n-1]`.

---

# Complexity

* **Time Complexity:** `O(n^2)`
* **Space Complexity:** `O(n^2)`

---

# Code

```java
import java.util.*;

public class Solution {
    public static int optimalStrategyOfGame(int[] coins, int n) {

        int[][] dp = new int[n][n];

        // Length 1 ranges (only one coin)
        for (int i = 0; i < n; i++) {
            dp[i][i] = coins[i];
        }

        // Fill DP table for increasing lengths
        for (int length = 2; length <= n; length++) {
            for (int i = 0; i + length - 1 < n; i++) {
                int j = i + length - 1;

                int pickLeft = coins[i] + Math.min(
                        (i + 2 <= j ? dp[i + 2][j] : 0),
                        (i + 1 <= j - 1 ? dp[i + 1][j - 1] : 0)
                );

                int pickRight = coins[j] + Math.min(
                        (i + 1 <= j - 1 ? dp[i + 1][j - 1] : 0),
                        (i <= j - 2 ? dp[i][j - 2] : 0)
                );

                dp[i][j] = Math.max(pickLeft, pickRight);
            }
        }

        return dp[0][n - 1];
    }
};

```

---

# Example Walkthrough

**Input:**

```
coins = [5, 30, 4, 1]
```

**DP reasoning:**

* Base cases: single coins → dp[i][i] = [5,30,4,1]

For subarrays of length 2:

* dp[0][1] = max(5, 30) = 30
* dp[1][2] = max(30, 4) = 30
* dp[2][3] = max(4, 1) = 4

For length 3 and 4 we fill using recurrence. Final computed value:

```
dp[0][3] = 31
```

Meaning the maximum guaranteed amount you can win is **31**.
