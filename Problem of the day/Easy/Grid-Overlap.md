# Intuition

We are given two binary matrices `MAT1` and `MAT2` (size N×N).
We can **shift MAT1** in any of four directions — **left, right, up, down** — any number of times.

Our goal is to find the **maximum overlap** — the number of positions where both have `1`.

So instead of performing all shifts directly, we can think:

> “For each possible shift (dx, dy), count how many overlapping 1’s occur if MAT1 is shifted by (dx, dy).”

Here:

* `dx` = vertical shift (down is +ve, up is -ve)
* `dy` = horizontal shift (right is +ve, left is -ve)

We just need to simulate all valid shifts and compute maximum overlap.

---

# Approach

1. Loop through all possible shifts:

   * `dx` in range `[-(n-1), n-1]`
   * `dy` in range `[-(n-1), n-1]`
2. For each shift `(dx, dy)`:

   * Compute overlap count:

     * For each `(i, j)` in `MAT1`, check if the shifted cell `(i + dx, j + dy)` in `MAT2` exists and is `1`.
     * If both are `1`, increase count.
3. Keep track of the **maximum count**.

---

# Complexity

* **Time:** O(N³)
  (Because we try (2N)² ≈ 4N² shifts and each shift checks N² cells)
* **Space:** O(1)
  (Only uses a few integer variables)

Given constraints (N ≤ 30), this is perfectly fine.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int gridOverlap(ArrayList<ArrayList<Integer>> mat1, ArrayList<ArrayList<Integer>> mat2, int n) {
        int maxOverlap = 0;

        for (int dx = -(n - 1); dx <= n - 1; dx++) {
            for (int dy = -(n - 1); dy <= n - 1; dy++) {
                int overlap = 0;

                for (int i = 0; i < n; i++) {
                    for (int j = 0; j < n; j++) {
                        int x = i + dx;
                        int y = j + dy;

                        if (x >= 0 && x < n && y >= 0 && y < n) {
                            if (mat1.get(i).get(j) == 1 && mat2.get(x).get(y) == 1) {
                                overlap++;
                            }
                        }
                    }
                }

                maxOverlap = Math.max(maxOverlap, overlap);
            }
        }

        return maxOverlap;
    }

    // Example test
    public static void main(String[] args) {
        ArrayList<ArrayList<Integer>> mat1 = new ArrayList<>();
        mat1.add(new ArrayList<>(Arrays.asList(1, 1, 0)));
        mat1.add(new ArrayList<>(Arrays.asList(0, 0, 0)));
        mat1.add(new ArrayList<>(Arrays.asList(0, 0, 0)));

        ArrayList<ArrayList<Integer>> mat2 = new ArrayList<>();
        mat2.add(new ArrayList<>(Arrays.asList(0, 1, 1)));
        mat2.add(new ArrayList<>(Arrays.asList(0, 0, 0)));
        mat2.add(new ArrayList<>(Arrays.asList(0, 0, 0)));

        System.out.println(gridOverlap(mat1, mat2, 3)); // Output: 2
    }
};

```

---
## Example 1

### Input:

```
3
MAT1:
1 1 0
0 0 0
0 0 0

MAT2:
0 1 1
0 0 0
0 0 0
```

We shift MAT1 one step **right**:

```
0 1 1
0 0 0
0 0 0
```

Now overlap with MAT2 = 2.
No other shift yields more.
→ Output: 2.

---
## Example 2

### Input:

```
MAT1:
1 1 0
0 1 0
0 1 0

MAT2:
0 0 0
0 1 1
0 0 1
```

Try different shifts:

* Shift down → overlap = 3 (best case)
* Shift up or sideways → less overlap.

**Answer: 3**

---
