## **Problem: Count Equal 0s, 1s, and 2s Subarrays**

We are given an array of only 0s, 1s, and 2s, and we need to count the number of **non-empty contiguous subarrays** that contain the **same number of 0s, 1s, and 2s**.

---

# Intuition

We need to identify subarrays where the **count of 0s = count of 1s = count of 2s**.

A brute force approach would consider all possible subarrays and check the counts, which would be **O(N³)**. But we aim to solve it in **linear time**.

Instead, use **prefix differences**:
- Maintain counters: `count0`, `count1`, `count2`.
- Let’s define a pair:
  - `diff1 = count1 - count0`
  - `diff2 = count2 - count1`

If the same pair `(diff1, diff2)` is seen again at a different index, the subarray between these indices has equal numbers of 0s, 1s, and 2s.

---

# Approach

1. Initialize:
   - `Map<Pair<Integer, Integer>, Integer>` to store frequency of `(diff1, diff2)`
   - Initialize map with `(0, 0) → 1`
2. Traverse array and update count of 0, 1, 2.
3. Compute `diff1 = count1 - count0`, `diff2 = count2 - count1`.
4. Add to result the current count of the pair in the map.
5. Update map with new frequency.

---
# Complexity Analysis

| Metric           | Value  |
| ---------------- | ------ |
| Time Complexity  | `O(N)` |
| Space Complexity | `O(N)` |

---

# Code

```java
import java.util.*;

public class Solution {
    public static int countSubarrays(int n, int[] arr) {
        Map<String, Integer> map = new HashMap<>();
        int count0 = 0, count1 = 0, count2 = 0, res = 0;
        map.put("0#0", 1); // Base case: before starting

        for (int i = 0; i < n; i++) {
            if (arr[i] == 0) count0++;
            else if (arr[i] == 1) count1++;
            else count2++;

            int d1 = count1 - count0;
            int d2 = count2 - count1;
            String key = d1 + "#" + d2;

            res += map.getOrDefault(key, 0);
            map.put(key, map.getOrDefault(key, 0) + 1);
        }

        return res;
    }
}
```

---

## Example Walkthrough

Let’s walk through `arr = [1, 1, 0, 2, 1]`.

| Index | Count0 | Count1 | Count2 | diff1 = c1 - c0 | diff2 = c2 - c1 | Pair (d1,d2) | Count in map | Result |
|-------|--------|--------|--------|------------------|------------------|---------------|----------------|--------|
| -1    |   0    |   0    |   0    |        0         |        0         |     (0,0)     |       1        |   0    |
|  0    |   0    |   1    |   0    |        1         |       -1         |     (1,-1)    |       0        |   0    |
|  1    |   0    |   2    |   0    |        2         |       -2         |     (2,-2)    |       0        |   0    |
|  2    |   1    |   2    |   0    |        1         |       -2         |     (1,-2)    |       0        |   0    |
|  3    |   1    |   2    |   1    |        1         |       -1         |     (1,-1)    |       1        |   1    |
|  4    |   1    |   3    |   1    |        2         |       -2         |     (2,-2)    |       1        |   2    |

Answer: **2**

---

## Sample Input & Output

**Input:**
```
2
5
1 1 0 2 1
4
1 1 0 0
```

**Output:**
```
2
0
```

