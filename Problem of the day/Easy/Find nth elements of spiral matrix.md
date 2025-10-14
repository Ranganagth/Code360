# Intuition

We need to find the **Kth element in the spiral order traversal** of a matrix.

A **spiral traversal** means:

1. Move left → right across the top row.
2. Move top → bottom along the right column.
3. Move right → left across the bottom row (if still within bounds).
4. Move bottom → top along the left column (if still within bounds).
5. Then move inward and repeat.

We keep track of **four boundaries**:

* `top` = first untraversed row index
* `bottom` = last untraversed row index
* `left` = first untraversed column index
* `right` = last untraversed column index

As we traverse, we decrement or increment these boundaries to spiral inward.

Once we have visited `k` elements, we can **return the kth element immediately** — no need to store all.

---

# Approach

1. Initialize:

   ```java
   int top = 0, bottom = n - 1;
   int left = 0, right = m - 1;
   int count = 0;
   ```
2. Traverse in layers:

   * Traverse from `left` → `right` (top row)
   * Traverse from `top + 1` → `bottom` (right column)
   * Traverse from `right - 1` → `left` (bottom row) if still within bounds
   * Traverse from `bottom - 1` → `top + 1` (left column) if still within bounds
3. After each element, increment `count`.

   * If `count == k`, **return that element immediately**.
4. Continue until all layers are done.

---

# Complexity

* **Time:** `O(N*M)` (in worst case we might traverse all elements)
* **Space:** `O(1)` (constant extra space)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int findKthElement(int[][] matrix, int k) {
        int n = matrix.length;
        int m = matrix[0].length;

        int top = 0, bottom = n - 1;
        int left = 0, right = m - 1;
        int count = 0;

        while (top <= bottom && left <= right) {

            // Traverse top row
            for (int j = left; j <= right; j++) {
                count++;
                if (count == k) return matrix[top][j];
            }
            top++;

            // Traverse right column
            for (int i = top; i <= bottom; i++) {
                count++;
                if (count == k) return matrix[i][right];
            }
            right--;

            // Traverse bottom row (if still within bounds)
            if (top <= bottom) {
                for (int j = right; j >= left; j--) {
                    count++;
                    if (count == k) return matrix[bottom][j];
                }
                bottom--;
            }

            // Traverse left column (if still within bounds)
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    count++;
                    if (count == k) return matrix[i][left];
                }
                left++;
            }
        }

        return -1; // Should not reach here if k is valid
    }

    // Example test
    public static void main(String[] args) {
        int[][] mat1 = {
            {1, 2, 3, 4},
            {5, 6, 7, 8},
            {7, 9, 2, 1}
        };
        System.out.println(findKthElement(mat1, 8)); // Output: 9

        int[][] mat2 = {
            {1, 2, 3, 4},
            {5, 6, 7, 8},
            {9, 10, 11, 12},
            {13, 14, 15, 16}
        };
        System.out.println(findKthElement(mat2, 10)); // Output: 13
    }
}
```

---

## Example

### Input:

```
3 4 8
1 2 3 4
5 6 7 8
7 9 2 1
```

**Spiral order:**
`1, 2, 3, 4, 8, 1, 2, 9, 7, 5, 6, 7`

8th element = `9`

---

## Walkthrough for Sample 2

Matrix:

```
1  2  3  4
5  6  7  8
9 10 11 12
13 14 15 16
```

Spiral traversal:
`1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10`

10th element = `13`

Output: `13`

---

