# Intuition

We need to traverse the matrix diagonally in a **zig-zag manner**:

* Start from `(0,0)` → go right until you finish the diagonal.
* On each diagonal, alternate the direction:

  * Even diagonals (0,2,4,...) → traverse **upwards-right**.
  * Odd diagonals (1,3,5,...) → traverse **downwards-left**.

Every element belongs to a diagonal identified by `(row + col)`.

---

# Approach

1. The total number of diagonals = `n + m - 1`.
2. For each diagonal `d`:

   * Find the **starting cell**:

     * If `d < m` → start from `(0, d)` (top row).
     * Else → start from `(d - m + 1, m - 1)` (rightmost column).
   * Collect all elements of this diagonal by moving `row++`, `col--` until out of bounds.
3. If the diagonal index `d` is **even**, reverse the collected elements (since traversal is upwards-right).
4. Append to result list.

---

# Complexity

* **Time:** `O(n*m)` → every cell is visited exactly once.
* **Space:** `O(n*m)` for the result list.

---

# Code

```java
import java.util.*;

public class Solution {

    public static ArrayList<Integer> printMatrix(int[][] mat, int n, int m) {
        ArrayList<Integer> result = new ArrayList<>();

        // Total diagonals = n + m - 1
        for (int d = 0; d < n + m - 1; d++) {
            ArrayList<Integer> diagonal = new ArrayList<>();

            // Starting row and col
            int row = (d < m) ? 0 : d - m + 1;
            int col = (d < m) ? d : m - 1;

            // Collect diagonal
            while (row < n && col >= 0) {
                diagonal.add(mat[row][col]);
                row++;
                col--;
            }

            // Reverse if diagonal index is even
            if (d % 2 == 0) {
                Collections.reverse(diagonal);
            }

            result.addAll(diagonal);
        }

        return result;
    }
}
```

---

## Example Walkthrough

For `mat = [[1,2,3],[4,5,6],[7,8,9]]`:

* Diagonal 0 → `[1]` → keep → `[1]`
* Diagonal 1 → `[2,4]` → already down-left → `[2,4]`
* Diagonal 2 → `[3,5,7]` → reverse → `[7,5,3]`
* Diagonal 3 → `[6,8]` → keep → `[6,8]`
* Diagonal 4 → `[9]` → reverse → `[9]`

Final = `[1,2,4,7,5,3,6,8,9]`.

---
