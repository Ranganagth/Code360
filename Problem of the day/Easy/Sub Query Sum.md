# Approach:

1. **Build a prefix sum matrix** `prefix[i][j]`, where each cell represents the sum of the submatrix from (0,0) to (i,j).
2. For a query `(r1, c1, r2, c2)`, compute the sum using the inclusion-exclusion formula:

   ```
   sum = prefix[r2][c2]
         - (r1 > 0 ? prefix[r1-1][c2] : 0)
         - (c1 > 0 ? prefix[r2][c1-1] : 0)
         + (r1 > 0 && c1 > 0 ? prefix[r1-1][c1-1] : 0)
   ```

---

# Code:

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> findSubmatrixSum(ArrayList<ArrayList<Integer>> arr, ArrayList<ArrayList<Integer>> queries) {
        int n = arr.size();
        int m = arr.get(0).size();

        // Step 1: Create prefix sum matrix
        int[][] prefix = new int[n][m];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int val = arr.get(i).get(j);
                int top = i > 0 ? prefix[i - 1][j] : 0;
                int left = j > 0 ? prefix[i][j - 1] : 0;
                int diag = (i > 0 && j > 0) ? prefix[i - 1][j - 1] : 0;

                prefix[i][j] = val + top + left - diag;
            }
        }

        // Step 2: Answer each query using prefix matrix
        ArrayList<Integer> result = new ArrayList<>();

        for (ArrayList<Integer> q : queries) {
            int r1 = q.get(0);
            int c1 = q.get(1);
            int r2 = q.get(2);
            int c2 = q.get(3);

            int total = prefix[r2][c2];
            if (r1 > 0) total -= prefix[r1 - 1][c2];
            if (c1 > 0) total -= prefix[r2][c1 - 1];
            if (r1 > 0 && c1 > 0) total += prefix[r1 - 1][c1 - 1];

            result.add(total);
        }

        return result;
    }
}
```

---

### Example:

For:

```java
ARR = [[1 , 2 , 3], [3 , 4 , 1], [2 , 1 , 2]]
Queries = [[0 , 0 , 1 , 2]]
```

Output:

```
[14]
```

---
