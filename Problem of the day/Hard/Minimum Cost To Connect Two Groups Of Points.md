# Intuition

Every point in group-1 must connect to **at least one** point in group-2, and every point in group-2 must also be connected at least once.
Since `N, M ≤ 10`, use **bitmask DP** to track which nodes of group-2 are already connected while processing nodes of group-1.

Let:

```
dp[i][mask] = minimum cost after connecting first i points of group-1
              and the set of connected points in group-2 equals mask
```

Transition: when processing point `i`, connect it to any `j` in group-2.

After all `N` points are processed, some points of group-2 may still be unconnected; connect those with the cheapest available connection.

---

# Approach

1. Precompute for each column `j`:

   ```
   minCol[j] = minimum cost[i][j] over all i
   ```

   (used to connect remaining uncovered points).

2. DP over rows:

   ```
   dp[0][0] = 0
   For each i:
       for each mask:
           for each j:
               newMask = mask | (1<<j)
               dp[i+1][newMask] =
                   min(dp[i+1][newMask],
                       dp[i][mask] + cost[i][j])
   ```

3. Final answer:
   For every mask after processing all rows,
   add the cost of connecting uncovered columns using `minCol`.

---

# Complexity

* States: `N * 2^M`
* Transition per state: `M`
* **Time:** `O(N * M * 2^M)` ≤ ~10000
* **Space:** `O(2^M)`

---

# Code

```java
import java.util.*;

public class Solution {

    public static int connectTwoGroups(int[][] cost) {
        int n = cost.length;
        int m = cost[0].length;
        int MAX = 1 << m;

        int[] minCol = new int[m];
        Arrays.fill(minCol, Integer.MAX_VALUE);

        for (int j = 0; j < m; j++) {
            for (int i = 0; i < n; i++) {
                minCol[j] = Math.min(minCol[j], cost[i][j]);
            }
        }

        int[][] dp = new int[n + 1][MAX];
        for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);
        dp[0][0] = 0;

        for (int i = 0; i < n; i++) {
            for (int mask = 0; mask < MAX; mask++) {
                if (dp[i][mask] == Integer.MAX_VALUE) continue;

                for (int j = 0; j < m; j++) {
                    int newMask = mask | (1 << j);
                    dp[i + 1][newMask] = Math.min(
                        dp[i + 1][newMask],
                        dp[i][mask] + cost[i][j]
                    );
                }
            }
        }

        int ans = Integer.MAX_VALUE;

        for (int mask = 0; mask < MAX; mask++) {
            if (dp[n][mask] == Integer.MAX_VALUE) continue;

            int extra = 0;
            for (int j = 0; j < m; j++) {
                if ((mask & (1 << j)) == 0) {
                    extra += minCol[j];
                }
            }

            ans = Math.min(ans, dp[n][mask] + extra);
        }

        return ans;
    }
};

```

---

# Example Walkthrough

Example

```
N = 3, M = 2
cost =
1 2
2 3
4 1
```

`minCol = [1,1]`

DP connects each row while marking which columns are already connected.
Best mask after processing all rows is `{0,1}` with cost `4`.
No uncovered column remains → final answer `4`.
