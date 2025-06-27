# Intuition:

* You can **shift mat1 in any direction**, which is effectively the same as **translating mat1 over mat2** in all possible directions.
* For each translation (i.e., offset `dx, dy`), you can compute the **number of overlapping 1s** where both `mat1` and `mat2` have 1s at the same position.
* Since `N <= 30`, it's feasible to try **all possible shifts** in both directions (up to ±(N - 1)) and calculate the overlaps.

---

# Approach:

1. For every possible shift `(dx, dy)` where `-N < dx, dy < N`:

   * Shift `mat1` by `(dx, dy)`
   * Count overlapping 1s between shifted `mat1` and `mat2`.
2. Return the **maximum overlap count** over all possible shifts.

---

# Complexity:

* `O(N^4)` in worst case

  * `O(N^2)` for looping all shifts
  * `O(N^2)` for comparing overlapping cells per shift
* Acceptable for `N <= 30`

---

# Code:

```java
import java.util.*;

public class Solution {
    public static int gridOverlap(ArrayList<ArrayList<Integer>> mat1, ArrayList<ArrayList<Integer>> mat2, int n) {
        int maxOverlap = 0;

        // Try all possible shifts in both x and y directions
        for (int dx = -n + 1; dx < n; dx++) {
            for (int dy = -n + 1; dy < n; dy++) {
                int overlap = countOverlap(mat1, mat2, dx, dy, n);
                maxOverlap = Math.max(maxOverlap, overlap);
            }
        }

        return maxOverlap;
    }

    // Helper function to count overlaps when mat1 is shifted by (dx, dy)
    private static int countOverlap(ArrayList<ArrayList<Integer>> mat1, ArrayList<ArrayList<Integer>> mat2, int dx, int dy, int n) {
        int count = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int x = i + dx;
                int y = j + dy;

                if (x >= 0 && x < n && y >= 0 && y < n) {
                    if (mat1.get(i).get(j) == 1 && mat2.get(x).get(y) == 1) {
                        count++;
                    }
                }
            }
        }

        return count;
    }
}
```

---

### Example:

For input:

```
mat1:
1 1 0
0 0 0
0 0 0

mat2:
0 1 1
0 0 0
0 0 0
```

After one **right shift**, `mat1` becomes:

```
0 1 1
0 0 0
0 0 0
```

Overlap with `mat2`: **2**

---
