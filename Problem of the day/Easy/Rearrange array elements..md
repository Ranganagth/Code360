# Intuition

We need to rearrange array elements based on their **absolute difference** from a given number `m`.

* Elements closer to `m` should appear **earlier**.
* If two elements have the same difference from `m`, **preserve their original order** (this means we need a *stable sort*).

This can be visualized as sorting the array based on the key:

```
|A[i] - m|
```

---

# Approach

1. Compute the absolute difference for each element:

   ```
   diff[i] = |A[i] - m|
   ```
2. Pair each element with its original index (for stability tracking):

   ```
   (A[i], diff[i], originalIndex)
   ```
3. Sort these pairs by:

   * **Primary key:** difference (`diff[i]`)
   * **Secondary key:** original index (to maintain order for equal differences)
4. Extract and return the rearranged elements.

---

## Example Walkthrough

### Example:

```
A = [3, 5, 7, 9, 2, 6], m = 5
```

| Element | |A[i] - m| | Index |
|----------|-----------|--------|
| 3        | 2         | 0      |
| 5        | 0         | 1      |
| 7        | 2         | 2      |
| 9        | 4         | 3      |
| 2        | 3         | 4      |
| 6        | 1         | 5      |

Sort by difference (and by index if equal):

```
(5, 0, 1)
(6, 1, 5)
(3, 2, 0)
(7, 2, 2)
(2, 3, 4)
(9, 4, 3)
```

**Rearranged Array:**
`[5, 6, 3, 7, 2, 9]`

---

# Complexity Analysis

* **Time Complexity:** O(N log N) (due to sorting)
* **Space Complexity:** O(N) (for storing pairs)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int[] rearrangeArray(int[] A, int m) {
        int n = A.length;

        // Create a list of triplets (value, difference, index)
        List<int[]> list = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            int diff = Math.abs(A[i] - m);
            list.add(new int[]{A[i], diff, i});
        }

        // Sort by difference first, then by original index
        Collections.sort(list, (a, b) -> {
            if (a[1] != b[1])
                return Integer.compare(a[1], b[1]);
            else
                return Integer.compare(a[2], b[2]);
        });

        // Extract sorted values
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = list.get(i)[0];
        }

        return result;
    }
};

```

---

## Example Run

### Input:

```
A = [3, 8, 1, 4]
m = 6
```

### Output:

```
[8, 4, 3, 1]
```

### Input:

```
A = [5, 5]
m = 12
```

### Output:

```
[5, 5]
```

---
