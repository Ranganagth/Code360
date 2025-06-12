# Intuition

To solve the problem of finding the **longest increasing path from cell (0, 0)** in a 2D matrix where moves are only allowed **to the right (→)** or **down (↓)** if the next cell has a strictly greater value, we can use **Dynamic Programming with Memoization**.

# Key Observations

* From any cell `(i, j)`, you can move to:
  * `(i, j + 1)` **if** `mat[i][j + 1] > mat[i][j]`
  * `(i + 1, j)` **if** `mat[i + 1][j] > mat[i][j]`
  
* We only move **right** or **down**, and only to a strictly **greater** value.

# Approach

1. Use a helper function with memoization (`dp[i][j]`) to store the longest path starting at cell `(i, j)`.
2. For each cell, check if moving right or down is valid and recursively get the maximum path.
3. Start from `(0, 0)` and compute the longest path from there.

# Complexity Analysis

* **Time:** `O(N * M)` – each cell is visited once due to memoization.
* **Space:** `O(N * M)` – for the DP table.

# Code

```java
import java.util.*;

public class Solution {
    public static int longestIncreasingPath(ArrayList<ArrayList<Integer>> mat, int n, int m) {
        int[][] dp = new int[n][m];
        for (int[] row : dp)
            Arrays.fill(row, -1);
        return dfs(0, 0, mat, n, m, dp);
    }

    private static int dfs(int i, int j, ArrayList<ArrayList<Integer>> mat, int n, int m, int[][] dp) {
        if (dp[i][j] != -1)
            return dp[i][j];

        int maxPath = 1;

        // Move Right
        if (j + 1 < m && mat.get(i).get(j + 1) > mat.get(i).get(j)) {
            maxPath = Math.max(maxPath, 1 + dfs(i, j + 1, mat, n, m, dp));
        }

        // Move Down
        if (i + 1 < n && mat.get(i + 1).get(j) > mat.get(i).get(j)) {
            maxPath = Math.max(maxPath, 1 + dfs(i + 1, j, mat, n, m, dp));
        }

        dp[i][j] = maxPath;
        return dp[i][j];
    }
}
```

---

