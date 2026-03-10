# Intuition

A cell is illuminated if there exists at least one bulb in:

* the same **row**
* the same **column**
* the same **main diagonal** `(r - c)`
* the same **anti-diagonal** `(r + c)`

Instead of simulating the entire matrix, maintain counts of active bulbs affecting rows, columns, and diagonals.

When answering a query:

```
if rowCount[r] > 0
or colCount[c] > 0
or diagCount[r-c] > 0
or antiDiagCount[r+c] > 0
```

then the cell is illuminated.

After answering, turn OFF any bulb in the **3×3 region centered at the query cell**.

---

# Approach

Data structures:

* `HashMap<Integer,Integer> row`
* `HashMap<Integer,Integer> col`
* `HashMap<Integer,Integer> diag`
* `HashMap<Integer,Integer> antiDiag`
* `HashSet<Long> activeBulbs`

Bulb position encoded as:

```
key = r * N + c
```

Steps:

1. Insert bulbs (avoid duplicates).
2. Update row/col/diag/antiDiag counts.
3. For each query:

   * check illumination
   * add result
   * check 9 surrounding cells and remove bulbs if present
   * update counts.

---

# Complexity

* **Time complexity:** (O(M + Q))
* **Space complexity:** (O(M))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static ArrayList<Integer> isIlluminated(int n, int m, int[][] bulb, int q, int[][] query) {

		HashMap<Integer,Integer> row = new HashMap<>();
		HashMap<Integer,Integer> col = new HashMap<>();
		HashMap<Integer,Integer> diag = new HashMap<>();
		HashMap<Integer,Integer> antiDiag = new HashMap<>();

		HashSet<Long> set = new HashSet<>();

		for (int[] b : bulb) {
			int r = b[0], c = b[1];
			long key = (long)r * n + c;

			if (set.contains(key)) continue;

			set.add(key);

			row.put(r, row.getOrDefault(r,0)+1);
			col.put(c, col.getOrDefault(c,0)+1);
			diag.put(r-c, diag.getOrDefault(r-c,0)+1);
			antiDiag.put(r+c, antiDiag.getOrDefault(r+c,0)+1);
		}

		ArrayList<Integer> ans = new ArrayList<>();

		int[][] dirs = {
			{0,0},{1,0},{-1,0},{0,1},{0,-1},
			{1,1},{1,-1},{-1,1},{-1,-1}
		};

		for (int[] qu : query) {

			int r = qu[0], c = qu[1];

			if (row.getOrDefault(r,0)>0 ||
				col.getOrDefault(c,0)>0 ||
				diag.getOrDefault(r-c,0)>0 ||
				antiDiag.getOrDefault(r+c,0)>0) {

				ans.add(1);
			} else ans.add(0);

			for (int[] d : dirs) {

				int nr = r + d[0];
				int nc = c + d[1];

				if (nr<0 || nc<0 || nr>=n || nc>=n) continue;

				long key = (long)nr * n + nc;

				if (!set.contains(key)) continue;

				set.remove(key);

				row.put(nr, row.get(nr)-1);
				col.put(nc, col.get(nc)-1);
				diag.put(nr-nc, diag.get(nr-nc)-1);
				antiDiag.put(nr+nc, antiDiag.get(nr+nc)-1);
			}
		}

		return ans;
	}
};

```

---

# Example walkthrough with explanation

Matrix size `3`

Bulb:

```
(0,0)
```

Illuminates:

```
row 0
col 0
diag 0
antiDiag 0
```

Query `(0,1)`:

```
row[0] > 0 → illuminated
```

Answer:

```
1
```

Then bulbs in surrounding 3×3 region are removed.

Matrix becomes fully dark.

Final result:

```
[1]
```
