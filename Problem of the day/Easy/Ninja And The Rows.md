# Intuition

The weight of each row is simply the **sum of its elements**.
The task reduces to computing the sum of every row and returning the **maximum** among them.

---
# Approach

1. Initialize `maxSum = 0`.
2. For each row:

   * compute sum of all elements in that row.
   * update `maxSum = max(maxSum, rowSum)`.
3. Return `maxSum`.

---

# Complexity

* **Time complexity:** (O(N \times M))
* **Space complexity:** $(O(1))$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int maximumWeightRow(int n, int m, int[][] mat) {

        int maxSum = 0;

        for (int i = 0; i < n; i++) {

            int rowSum = 0;

            for (int j = 0; j < m; j++) {
                rowSum += mat[i][j];
            }

            maxSum = Math.max(maxSum, rowSum);
        }

        return maxSum;
    }
};

```

---

# Example walkthrough

Input:

```
mat = [
 [1,2,3],
 [2,0,0]
]
```

Row sums:

```
Row 1 → 6
Row 2 → 2
```

Maximum:

```
6
```
