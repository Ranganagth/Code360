## Problem Understanding

This problem is like a **"Candy Crush" style elimination game** on a 2D grid.
We need to repeatedly:

1. Detect bombs that should blast (≥3 adjacent horizontally or vertically with the same value).
2. Blast them (set to `0`).
3. Apply gravity (drop bombs downward).
4. Repeat until stable.

---

# Intuition

* Each round: scan the grid → mark groups of bombs → clear them → gravity → repeat.
* Stop when a full scan finds **no blasts**.

---

# Approach

1. **Detect blasts:**

   * For each cell `bombGrid[i][j] != 0`, check horizontally and vertically if there are ≥3 consecutive same values.
   * Use a boolean `toBlast[n][m]` to mark.
2. **Blast:**

   * Set all marked cells → `0`.
3. **Gravity:**

   * For each column, collect all nonzero values from bottom up and drop them down.
   * Fill remaining top cells with `0`.
4. Repeat until no changes.

---

# Complexity

* Each iteration scans the full grid: **O(n × m)**.
* Worst-case, many blasts may occur, but bounded by number of bombs (≤ N×M ≤ 2500).
* Total is efficient for N, M ≤ 50.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<ArrayList<Integer>> getSafeState(ArrayList<ArrayList<Integer>> bombGrid, int n, int m) {
        boolean changed = true;

        while (changed) {
            changed = false;
            boolean[][] toBlast = new boolean[n][m];

            // Step 1: Mark blasts
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    int val = bombGrid.get(i).get(j);
                    if (val == 0) continue;

                    // horizontal check
                    int count = 1;
                    int k = j + 1;
                    while (k < m && bombGrid.get(i).get(k) == val) {
                        count++;
                        k++;
                    }
                    if (count >= 3) {
                        for (int t = 0; t < count; t++) {
                            toBlast[i][j + t] = true;
                        }
                    }

                    // vertical check
                    count = 1;
                    k = i + 1;
                    while (k < n && bombGrid.get(k).get(j) == val) {
                        count++;
                        k++;
                    }
                    if (count >= 3) {
                        for (int t = 0; t < count; t++) {
                            toBlast[i + t][j] = true;
                        }
                    }
                }
            }

            // Step 2: Blast (set to 0)
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < m; j++) {
                    if (toBlast[i][j]) {
                        bombGrid.get(i).set(j, 0);
                        changed = true;
                    }
                }
            }

            // Step 3: Gravity
            for (int col = 0; col < m; col++) {
                int writeRow = n - 1;
                for (int row = n - 1; row >= 0; row--) {
                    if (bombGrid.get(row).get(col) != 0) {
                        bombGrid.get(writeRow).set(col, bombGrid.get(row).get(col));
                        if (writeRow != row) bombGrid.get(row).set(col, 0);
                        writeRow--;
                    }
                }
            }
        }

        return bombGrid;
    }
};

```

---

### Walkthrough Example

Input:

```
3 3
1 1 1
1 2 1
1 1 1
```

1. First scan:

   * Entire border of `1`s horizontally & vertically → blasted.
   * Grid becomes:

   ```
   0 0 0
   0 2 0
   0 0 0
   ```
2. Gravity:

   * `2` drops to bottom:

   ```
   0 0 0
   0 0 0
   0 2 0
   ```
3. No more blasts → Safe grid.

Output:

```
0 0 0
0 0 0
0 2 0
```

---
