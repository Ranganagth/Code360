# Intuition

You want the longest *simple* path from source to destination — simple means you cannot revisit a cell. Longest simple path in grids is inherently exponential. The grid is tiny, so brute-force backtracking works. The only safe strategy: explore *all* possible routes using DFS, mark a cell as visited while exploring it, and unmark when backtracking. Track the maximum length that actually reaches the destination.

---

# Approach

1. If source or destination is blocked → return -1.
2. Maintain a boolean `visited[n][m]`.
3. DFS from `(sx, sy)` with current path length.
4. At each step:
   * If you reached `(dx, dy)` → update answer and stop deeper exploration from here.
   * Try 4 directions.
   * Move only if inside bounds, cell is 1, and not visited.
5. Mark before recursing, unmark after (backtracking).
6. If no route reaches destination, return -1.

---

# Time Complexity

- **Worst case:** `O(4^(N*M))`
- **Space:** `O(N*M)` recursion + visited array

---

# Code

```java
import java.util.*;

public class Solution {

    static int ans;
    static int[] dr = {-1, 1, 0, 0};
    static int[] dc = {0, 0, -1, 1};

    public static int longestPath(int n, int m, int[][] mat,
                                  int sx, int sy, int dx, int dy) {

        if (mat[sx][sy] == 0 || mat[dx][dy] == 0) return -1;

        boolean[][] visited = new boolean[n][m];
        ans = -1;

        dfs(mat, n, m, sx, sy, dx, dy, visited, 0);

        return ans;
    }

    private static void dfs(int[][] mat, int n, int m,
                            int r, int c, int dx, int dy,
                            boolean[][] visited, int len) {

        if (r == dx && c == dy) {
            ans = Math.max(ans, len);
            return;
        }

        visited[r][c] = true;

        for (int k = 0; k < 4; k++) {
            int nr = r + dr[k];
            int nc = c + dc[k];

            if (nr >= 0 && nr < n && nc >= 0 && nc < m &&
                mat[nr][nc] == 1 && !visited[nr][nc]) {

                dfs(mat, n, m, nr, nc, dx, dy, visited, len + 1);
            }
        }

        visited[r][c] = false;
    }
};

```

---

# Example walkthrough

**Matrix**
1 1 0
0 1 1
1 0 1

**Source:** (0,0)
**Destination:** (2,2)

**DFS explores:**

(0,0) → (0,1) → (1,1) → (1,2) → (2,2)

No other legal continuation exists without revisiting cells.
**Path length** = 4.
No longer path reaches destination.
**Answer** = 4

---