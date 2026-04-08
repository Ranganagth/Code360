# Intuition

Constraints define **placement conflicts**:

* Same row: no adjacent (left/right)
* Previous row: no upper-left / upper-right

This is a **row-by-row placement problem with state dependency**.

Each row can be represented as a **bitmask**:

* 1 → house placed
* 0 → empty

Goal:
Maximize total houses while ensuring:

* Valid row configuration
* No conflict with previous row

This becomes **DP with bitmasking**.

---

# Approach

1. Represent each row as a bitmask of length `n`
2. For each row:

   * Iterate all possible masks (0 → (2^n - 1))
   * Validate mask:

     * No house on 'T'
     * No adjacent bits (`mask & (mask << 1) == 0`)
3. Transition:

   * For current mask and previous mask:

     * Ensure no diagonal conflict:

       * `(mask & (prevMask << 1)) == 0`
       * `(mask & (prevMask >> 1)) == 0`
4. DP:

   * `dp[row][mask] = max houses till this row`
5. Optimize:

   * Use rolling array (`prevRow`, `currRow`)
6. Answer = max value in last row

---

# Complexity

* **Time complexity:**
  $$O(M \cdot 2^N \cdot 2^N)$$
  $(Since (N \leq 8), feasible)$

* **Space complexity:**
  $$O(2^N)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {

	public static int planTown(int m, int n, char rides[][]){

		int[] prevRow = new int[1 << n];
		Arrays.fill(prevRow, -1);
		prevRow[0] = 0;

		for (int row = 0; row < m; row++) {

			int[] currRow = new int[1 << n];
			Arrays.fill(currRow, -1);

			for (int mask = 0; mask < (1 << n); mask++) {

				if (!isValid(mask, rides[row])) continue;

				for (int prevMask = 0; prevMask < (1 << n); prevMask++) {

					if (prevRow[prevMask] == -1) continue;

					// diagonal conflict check
					if ((mask & (prevMask << 1)) != 0 || 
						(mask & (prevMask >> 1)) != 0) {
						continue;
					}

					int houses = Integer.bitCount(mask);
					currRow[mask] = Math.max(currRow[mask],
							prevRow[prevMask] + houses);
				}
			}

			prevRow = currRow;
		}

		int ans = 0;
		for (int val : prevRow) {
			ans = Math.max(ans, val);
		}

		return ans;
	}

	private static boolean isValid(int mask, char[] row) {

		// no house on tree
		for (int col = 0; col < row.length; col++) {
			if ((mask & (1 << col)) != 0 && row[col] == 'T') {
				return false;
			}
		}

		// no adjacent houses
		if ((mask & (mask << 1)) != 0) {
			return false;
		}

		return true;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Grid:
. . . T .
. T . . .
T . . T .

Row 0:
Valid masks avoid adjacent and 'T' at col 3
Possible placements → maximize houses

Row 1:
Avoid:

* Trees at col 1
* Diagonal conflicts from row 0

Row 2:
Continue transitions ensuring:

* No adjacency
* No diagonal conflicts

DP accumulates max placements

Final result = 8

## Example 2:

Grid:
T . . . .
T . T T .
. T . . T

Constraints restrict placements heavily

Optimal placements selected across rows
Final result = 5
