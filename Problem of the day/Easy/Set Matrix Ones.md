# Intuition

You are given a binary matrix `MAT`.
If any cell `(i, j)` has value `1`, then **entire row i** and **entire column j** should be changed to `1`.

However, you must be careful — if you modify the matrix directly while scanning, you might cause **unintended cascading updates**.

For example:

```
Initial:
0 1
0 0

If you modify row/column immediately when you see 1,
you’ll turn more 0’s into 1’s, which will then spread further.
```

So you need to **record which rows and columns should become 1 first**,
then modify the matrix afterward.

---

# Approach

1. Create two boolean arrays:

   * `row[]` of size `n`
   * `col[]` of size `m`

2. Traverse the matrix:

   * If `MAT[i][j] == 1`, mark:

     ```
     row[i] = true
     col[j] = true
     ```

3. Traverse again:

   * If either `row[i]` or `col[j]` is `true`, set:

     ```
     MAT[i][j] = 1
     ```

4. That’s it — you now have the modified matrix.

---

# Complexity Analysis

* **Time Complexity:** `O(n * m)`
  (We traverse the matrix twice)
* **Space Complexity:** `O(n + m)`
  (Extra arrays for rows and columns)

---

## Example Walkthrough

**Input:**

```
MAT = [
  [0, 0, 0],
  [0, 0, 1]
]
```

**Step 1:** Find cells with 1
→ (1, 2) is 1 → mark `row[1] = true`, `col[2] = true`

**Step 2:** Update matrix

* Every cell in row 1 or col 2 becomes 1.

**Result:**

```
[0, 0, 1]
[1, 1, 1]
```

Matches expected output.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static void setMatrixOnes(ArrayList<ArrayList<Integer>> MAT, int n, int m) {
        boolean[] row = new boolean[n];
        boolean[] col = new boolean[m];

        // Step 1: Mark rows and columns that need to be updated
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (MAT.get(i).get(j) == 1) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }

        // Step 2: Update matrix based on marked rows and columns
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (row[i] || col[j]) {
                    MAT.get(i).set(j, 1);
                }
            }
        }
    }
};

```

---

## Example Verification

**Input:**

```
2
2 2
1 0
0 0
1 2
0 1
```

**Output:**

```
1 1
1 0
1 1
```

Matches sample output.

---
