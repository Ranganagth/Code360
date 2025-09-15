# Intuition

* If we only had sum queries (no updates), we could preprocess a **2D prefix sum** in `O(N*M)` and answer each query in `O(1)`.
* But since updates are included, recomputing prefix sums after each update would be too slow (`O(N*M*Q)`).
* Instead, we need a **data structure that supports both updates and range sum queries efficiently**.
  The standard solution is a **2D Binary Indexed Tree (Fenwick Tree)**.

---

# Approach

1. **Fenwick Tree Setup**

   * Build a 2D Fenwick tree of size `N x M`.
   * It supports:

     * `update(r, c, delta)` in `O(logN * logM)`
     * `prefixSum(r, c)` = sum of rectangle `(0,0)` to `(r,c)` in `O(logN * logM)`

2. **Range Sum Query**

   * To find sum of submatrix `(r1,c1)` to `(r2,c2)`, we use inclusion-exclusion:

     ```
     sum(r1,c1,r2,c2) =
         prefixSum(r2,c2)
       - prefixSum(r1-1,c2)
       - prefixSum(r2,c1-1)
       + prefixSum(r1-1,c1-1)
     ```

3. **Handling Updates**

   * When updating `(r,c)` to new value `val`:

     * Compute `delta = val - grid[r][c]`
     * Call `update(r,c,delta)`
     * Update `grid[r][c] = val`

# Complexity

   * Building tree: `O(N*M*logN*logM)` (or `O(N*M)` if we just update each entry once).
   * Each query: `O(logN * logM)`
   * With constraints (`N,M ≤ 1000`, `Q ≤ 1e5`), this is efficient.

---

# Code

```java
import java.util.*;

public class Solution {
    static class Fenwick2D {
        int n, m;
        int[][] bit;

        Fenwick2D(int n, int m) {
            this.n = n;
            this.m = m;
            bit = new int[n + 1][m + 1]; // 1-based indexing
        }

        void update(int x, int y, int delta) {
            for (int i = x + 1; i <= n; i += i & -i) {
                for (int j = y + 1; j <= m; j += j & -j) {
                    bit[i][j] += delta;
                }
            }
        }

        int prefixSum(int x, int y) {
            int sum = 0;
            for (int i = x + 1; i > 0; i -= i & -i) {
                for (int j = y + 1; j > 0; j -= j & -j) {
                    sum += bit[i][j];
                }
            }
            return sum;
        }

        int rangeSum(int x1, int y1, int x2, int y2) {
            return prefixSum(x2, y2)
                 - (x1 > 0 ? prefixSum(x1 - 1, y2) : 0)
                 - (y1 > 0 ? prefixSum(x2, y1 - 1) : 0)
                 + (x1 > 0 && y1 > 0 ? prefixSum(x1 - 1, y1 - 1) : 0);
        }
    }

    public static int[] matrixRangeSum(int[][] grid, int[][] queries) {
        int n = grid.length, m = grid[0].length;
        Fenwick2D fenwick = new Fenwick2D(n, m);

        // Build BIT with initial grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                fenwick.update(i, j, grid[i][j]);
            }
        }

        List<Integer> result = new ArrayList<>();
        for (int[] q : queries) {
            if (q[0] == 1) { 
                // type-1 query: sum
                int x1 = q[1], y1 = q[2], x2 = q[3], y2 = q[4];
                result.add(fenwick.rangeSum(x1, y1, x2, y2));
            } else {
                // type-2 query: update
                int r = q[1], c = q[2], val = q[3];
                int delta = val - grid[r][c];
                fenwick.update(r, c, delta);
                grid[r][c] = val;
            }
        }

        return result.stream().mapToInt(i -> i).toArray();
    }
}
```

---

## Example Walkthrough

### Input:

```
GRID = [
  [7, 5, 3, 2]
]
Queries:
1 0 0 0 1   → sum(0,0)-(0,1)
2 0 0 2     → update(0,0)=2
1 0 0 0 1   → sum(0,0)-(0,1)
```

### Execution:

1. Query 1: sum = 7+5 = **12**
2. Query 2: grid becomes `[2,5,3,2]`
3. Query 3: sum = 2+5 = **7**

### Output:

```
12 7
```

---
