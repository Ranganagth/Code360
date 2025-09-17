# Intuition

To rotate a square matrix 90° clockwise:

1. The first column of the original becomes the first row of the rotated matrix.
2. The second column becomes the second row, and so on.

But instead of directly building a new matrix, we can **do it in-place** using two steps:

1. **Transpose** the matrix → convert rows into columns.
2. **Reverse each row** → gives the correct clockwise rotation.

---

# Approach

1. **Transpose step**
   Swap `nums[i][j]` with `nums[j][i]` for all `i < j`.
   This flips the matrix across its main diagonal.

2. **Reverse rows step**
   For each row, reverse the order of elements (swap first with last, second with second-last, etc.).

This ensures the matrix is rotated **90° clockwise in-place**.

---

# Complexity

* **Time:** `O(N^2)` (we touch each element at most twice: once in transpose, once in reverse).
* **Space:** `O(1)` extra (done in-place, no extra matrix required).

---
# Code

```java
import java.util.*; 
import java.io.*; 

public class Solution {
    static void rotateClockwise(int n, int[][] nums) {
        // Step 1: Transpose
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = nums[i][j];
                nums[i][j] = nums[j][i];
                nums[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                int temp = nums[i][left];
                nums[i][left] = nums[i][right];
                nums[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

---

## Example Walkthrough

Matrix:

```
1 2 3
4 5 6
7 8 9
```

1. **Transpose:**

```
1 4 7
2 5 8
3 6 9
```

2. **Reverse rows:**

```
7 4 1
8 5 2
9 6 3
```

Final rotated matrix = correct.

---

