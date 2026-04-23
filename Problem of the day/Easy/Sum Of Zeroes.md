# Intuition

For every cell with value `0`, count how many of its **4-directional neighbors** are `1`.

Total coverage = sum of such counts for all zero cells.

No need for complex structures. Direct grid traversal.

---

# Approach

1. Traverse each cell `(i, j)`
2. If `mat[i][j] == 0`:

   * Check 4 directions:

     * Up → `(i-1, j)`
     * Down → `(i+1, j)`
     * Left → `(i, j-1)`
     * Right → `(i, j+1)`
   * For each valid neighbor:

     * If value == 1 → increment count
3. Accumulate total coverage
4. Return result

---

# Complexity

* **Time complexity:**
  $$O(N \cdot M)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 
import java.util.ArrayList;

public class Solution {

	public static Integer coverageOfMatrix(ArrayList<ArrayList<Integer>> mat) {

		int n = mat.size();
		int m = mat.get(0).size();

		int total = 0;

		int[] dx = {-1, 1, 0, 0};
		int[] dy = {0, 0, -1, 1};

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {

				if (mat.get(i).get(j) == 0) {

					for (int d = 0; d < 4; d++) {
						int ni = i + dx[d];
						int nj = j + dy[d];

						if (ni >= 0 && nj >= 0 && ni < n && nj < m) {
							if (mat.get(ni).get(nj) == 1) {
								total++;
							}
						}
					}
				}
			}
		}

		return total;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Matrix:
1 0
0 1

Cell (0,1):

* Left → 1
* Down → 1
  Coverage = 2

Cell (1,0):

* Up → 1
* Right → 1
  Coverage = 2

Total = 2 + 2 = 4

---

## Example 2:

Matrix:
0 0 0
0 0 0

No adjacent 1s anywhere

Total = 0
