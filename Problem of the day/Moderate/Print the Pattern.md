# Solution 1

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	public static String[] NumberPattern(int n) {

		int[][] mat = new int[n][n];
		int value = 1;

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				mat[i][j] = value++;
			}
		}

		String[] result = new String[n];
		int index = 0;

		for (int i = 0; i < n; i += 2) {
			StringBuilder sb = new StringBuilder();
			for (int val : mat[i]) {
				sb.append(val).append(" ");
			}
			result[index++] = sb.toString().trim();
		}

		for (int i = (n % 2 == 0 ? n - 1 : n - 2); i >= 0; i-= 2) {
			StringBuilder sb = new StringBuilder();
			for (int val : mat[i]) {
				sb.append(val).append(" ");
			}
			result[index++] = sb.toString().trim();
		}

		return result;
	}

};

```

---

# Solution 2
## Core rule

For `n` rows and `n` columns:

* Numbers increase left to right.
* Each row contains `n` consecutive numbers.
* Row order follows this sequence of starting values:

  ```
  1
  1 + 2n
  1 + 4n
  ...
  (then remaining rows from bottom, stepping backward by 2n)
  ```

Equivalent formulation:

* First print rows with **even block index**
* Then print rows with **odd block index in reverse**

Block index = `rowIndex` in the logical construction, not matrix row.

---

### Direct formula (no matrix required)

For logical row `r` (0-based):

```
start = r * n + 1
row values = start .. start + n - 1
```

Print order:

* `r = 0, 2, 4, ...`
* then `r = lastOdd, ..., 1`

---

# Code

```java
public class Solution {
    public static String[] NumberPattern(int n) {

        String[] result = new String[n];
        int idx = 0;

        // even rows
        for (int r = 0; r < n; r += 2) {
            StringBuilder sb = new StringBuilder();
            int start = r * n + 1;
            for (int k = 0; k < n; k++) {
                sb.append(start + k);
                if (k < n - 1) sb.append(" ");
            }
            result[idx++] = sb.toString();
        }

        // odd rows in reverse
        int lastOdd = (n % 2 == 0) ? n - 1 : n - 2;
        for (int r = lastOdd; r >= 1; r -= 2) {
            StringBuilder sb = new StringBuilder();
            int start = r * n + 1;
            for (int k = 0; k < n; k++) {
                sb.append(start + k);
                if (k < n - 1) sb.append(" ");
            }
            result[idx++] = sb.toString();
        }

        return result;
    }
};

```

---

### Summary

* Preserves strict sequential numbering.
* Avoids unnecessary 2D storage.
* Matches all given samples exactly.
* Deterministic formula-based construction.
* Minimal operations, no rearrangement artifacts.

---