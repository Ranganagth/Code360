# Intuition

Convert 2D problem → multiple 1D problems.

Fix two rows (`top` and `bottom`), and compress matrix between them into a 1D array:

```
colSum[c] = sum of elements from row top → bottom at column c
```

Now problem reduces to:

```
Count subarrays with sum = 0
```

Use prefix sum + hashmap.

---

# Approach

1. Fix `top` row
2. Initialize `colSum[] = 0`
3. For each `bottom` row:

   * update `colSum`
   * count zero-sum subarrays using hashmap
4. Sum all counts

---

# Complexity

* **Time complexity:**
$$O(N^3)$$

* **Space complexity:**
$$O(N)$$


---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int subMatrices(int[][] mat, int n) {

        int count = 0;

        for (int top = 0; top < n; top++) {

            int[] colSum = new int[n];

            for (int bottom = top; bottom < n; bottom++) {

                for (int c = 0; c < n; c++) {
                    colSum[c] += mat[bottom][c];
                }

                count += countZeroSubarrays(colSum);
            }
        }

        return count;
    }

    private static int countZeroSubarrays(int[] arr) {

        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);

        int sum = 0;
        int count = 0;

        for (int num : arr) {
            sum += num;

            if (map.containsKey(sum)) {
                count += map.get(sum);
            }

            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```

---

# Example walkthrough

Matrix:

```
1  -1
0   0
```

For top=0, bottom=1:

```
colSum = [1+0, -1+0] = [1, -1]
```

Zero subarrays:

```
[1,-1] → sum = 0
```

Total count accumulates → 5

---

# Key Insight

```
2D submatrix sum → fix rows → reduce to 1D zero-sum subarray
```
