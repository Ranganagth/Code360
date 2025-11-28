# Intuition

Traverse the matrix column by column. For even-indexed columns, read values from top to bottom. For odd-indexed columns, read values from bottom to top. This alternating direction creates the sine-wave pattern.

---

# Approach

1. Create a result array of size `nRows * mCols`.
2. Loop through every column `col` from `0` to `mCols - 1`.
3. If the column index is even, iterate `row` from `0 → nRows - 1` and store values.
4. If the column index is odd, iterate `row` from `nRows - 1 → 0` and store values.
5. Return the final result array.

---

# Complexity

* **Time Complexity:** `O(nRows * mCols)` — every element is visited exactly once.
* **Space Complexity:** `O(nRows * mCols)` — output array.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int[] wavePrint(int arr[][], int nRows, int mCols) {
        int[] result = new int[nRows * mCols];
        int index = 0;

        for (int col = 0; col < mCols; col++) {
            if (col % 2 == 0) {
                // top to bottom
                for (int row = 0; row < nRows; row++) {
                    result[index++] = arr[row][col];
                }
            } else {
                // bottom to top
                for (int row = nRows - 1; row >= 0; row--) {
                    result[index++] = arr[row][col];
                }
            }
        }

        return result;
    }
};

```

---

# Example Walkthrough

Input matrix:

```
1  2  3  4
5  6  7  8
9 10 11 12
```

Traversal:

| Column | Direction    | Picked Values |
| ------ | ------------ | ------------- |
| 0      | Top → Bottom | 1, 5, 9       |
| 1      | Bottom → Top | 10, 6, 2      |
| 2      | Top → Bottom | 3, 7, 11      |
| 3      | Bottom → Top | 12, 8, 4      |

Final output:

```
[1, 5, 9, 10, 6, 2, 3, 7, 11, 12, 8, 4]
```
