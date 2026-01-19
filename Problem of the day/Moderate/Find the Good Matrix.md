# Intuition

A zero contaminates its entire row and column.
Directly modifying the matrix while scanning corrupts future decisions.
The zero locations must be identified first, then applied in a second pass.

---

# Approach

1. Scan the matrix once.
2. Record which rows and which columns contain at least one `0`.

   * Use two boolean arrays: `rowZero[N]`, `colZero[M]`.
3. Scan the matrix again.
4. If a cell lies in a marked row or marked column, set it to `0`.
5. Modify the matrix in place and return it.

This preserves original zero positions while ensuring correct propagation.

---

# Complexity

* **Time Complexity:** O(N × M)
* **Space Complexity:** O(N + M)

No extra matrix created.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static ArrayList<ArrayList<Integer>> findGoodMatrix(ArrayList<ArrayList<Integer>> arr) {

        int n = arr.size();
        int m = arr.get(0).size();

        boolean[] rowZero = new boolean[n];
        boolean[] colZero = new boolean[m];

        // First pass: detect zero rows and columns
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (arr.get(i).get(j) == 0) {
                    rowZero[i] = true;
                    colZero[j] = true;
                }
            }
        }

        // Second pass: update matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (rowZero[i] || colZero[j]) {
                    arr.get(i).set(j, 0);
                }
            }
        }

        return arr;
    }
};

```

---

# Example Walkthrough

**Input matrix**

```
1 0 1
1 1 1
1 1 1
```

**First pass**

* Zero found at (0,1)
* `rowZero[0] = true`
* `colZero[1] = true`

**Second pass**

* Entire row 0 → 0
* Entire column 1 → 0

**Result**

```
0 0 0
1 0 1
1 0 1
```

---

## Core Insight

- Zero propagation is a dependency problem.
- Separate detection from mutation to preserve correctness.
