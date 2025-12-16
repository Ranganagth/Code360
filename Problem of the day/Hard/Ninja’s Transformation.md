## Intuition

Each cell’s rank depends only on **relative ordering within its row and column**, not on global position.
Equal values connected through rows or columns must share the **same rank**.
Larger values must have **strictly larger ranks** than all smaller values in the same row or column.
The rank must be **minimal**, so it should be exactly `1 + max(rank of all smaller connected elements)`.

This is a **partial order** problem with equality constraints → solve by:

* grouping equal values,
* enforcing row/column ordering,
* assigning ranks incrementally from smallest values upward.

---

## Approach

1. **Flatten the matrix** into `(value, row, col)` cells.
2. **Sort all cells by value ascending**.
3. Process cells **grouped by equal values**:

   * For the current value, temporarily compute rank for each cell as:

     ```
     rank = 1 + max(rowRank[row], colRank[col])
     ```
   * But equal values in the same row/column must have the **same rank**.
4. To enforce equality:

   * Use **Union-Find (DSU)** within the group to connect cells sharing a row or column.
   * For each connected component, take the **maximum required rank** and assign it to all.
5. After finalizing a group:

   * Update `rowRank[row]` and `colRank[col]` with assigned ranks.
6. Continue to the next value group.

This guarantees:

* Order constraints are respected.
* Equal values share ranks.
* Ranks are minimal.

---

## Complexity

* Let `R = rows`, `C = cols`, `N = R * C`
* **Time:** `O(N log N)` (sorting + union-find)
* **Space:** `O(N + R + C)`

---

## Java Code

```java
import java.util.*;

public class Solution {

    static class Cell {
        int val, r, c;
        Cell(int v, int r, int c) {
            this.val = v;
            this.r = r;
            this.c = c;
        }
    }

    static class DSU {
        int[] parent;
        DSU(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) parent[i] = i;
        }
        int find(int x) {
            if (parent[x] != x) parent[x] = find(parent[x]);
            return parent[x];
        }
        void union(int a, int b) {
            int pa = find(a), pb = find(b);
            if (pa != pb) parent[pb] = pa;
        }
    }

    public static ArrayList<ArrayList<Integer>> ninjaTransformation(ArrayList<ArrayList<Integer>> matrix) {
        int R = matrix.size();
        int C = matrix.get(0).size();

        List<Cell> cells = new ArrayList<>();
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                cells.add(new Cell(matrix.get(i).get(j), i, j));
            }
        }

        cells.sort(Comparator.comparingInt(a -> a.val));

        int[][] ans = new int[R][C];
        int[] rowRank = new int[R];
        int[] colRank = new int[C];

        int i = 0;
        while (i < cells.size()) {
            int j = i;
            while (j < cells.size() && cells.get(j).val == cells.get(i).val) j++;

            int size = j - i;
            DSU dsu = new DSU(size);

            Map<Integer, Integer> rowMap = new HashMap<>();
            Map<Integer, Integer> colMap = new HashMap<>();

            for (int k = 0; k < size; k++) {
                Cell cell = cells.get(i + k);

                if (rowMap.containsKey(cell.r))
                    dsu.union(k, rowMap.get(cell.r));
                else
                    rowMap.put(cell.r, k);

                if (colMap.containsKey(cell.c))
                    dsu.union(k, colMap.get(cell.c));
                else
                    colMap.put(cell.c, k);
            }

            int[] compRank = new int[size];
            for (int k = 0; k < size; k++) {
                Cell cell = cells.get(i + k);
                int root = dsu.find(k);
                compRank[root] = Math.max(
                        compRank[root],
                        1 + Math.max(rowRank[cell.r], colRank[cell.c])
                );
            }

            for (int k = 0; k < size; k++) {
                int root = dsu.find(k);
                Cell cell = cells.get(i + k);
                ans[cell.r][cell.c] = compRank[root];
            }

            for (int k = 0; k < size; k++) {
                Cell cell = cells.get(i + k);
                rowRank[cell.r] = Math.max(rowRank[cell.r], ans[cell.r][cell.c]);
                colRank[cell.c] = Math.max(colRank[cell.c], ans[cell.r][cell.c]);
            }

            i = j;
        }

        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        for (int r = 0; r < R; r++) {
            ArrayList<Integer> row = new ArrayList<>();
            for (int c = 0; c < C; c++) row.add(ans[r][c]);
            result.add(row);
        }
        return result;
    }
}
```

---

## Example Walkthrough

Matrix:

```
1 2
3 4
```

Sorted values:

```
1 → (0,0)
2 → (0,1)
3 → (1,0)
4 → (1,1)
```

* `1`: no smaller neighbors → rank = 1
* `2`: larger than 1 in same row → rank = 2
* `3`: larger than 1 in same column → rank = 2
* `4`: larger than 2 and 3 → rank = 3

Result:

```
1 2
2 3
```

All constraints satisfied with minimal ranks.
