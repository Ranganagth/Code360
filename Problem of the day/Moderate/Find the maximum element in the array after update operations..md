# Intuition:

Brute-forcing every operation by looping over its whole range makes the total work O(N·M). With N, M ≤ 10³ it passes, but it’s the wrong habit. Use a difference array: mark the start of an increment with +1 and the position after the end with −1. A prefix sum then reconstructs final values in O(N). Each operation becomes O(1).

---

# Approach:

1. Create an array `diff` of size `n+2`, initialized to 0 (1-based indexing).
2. For every operation `[L, R]`:
   * `diff[L] += 1`
   * `diff[R+1] -= 1`
3. Build the final array by prefix-summing `diff`. Track the maximum while building.
4. Return that maximum.

---

# Complexity:

- **Time:** O(N + M)
- **Space:** O(N)

---

# Code:

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int findMaxElement(ArrayList<ArrayList<Integer>> arr, int n, int m) {
        int[] diff = new int[n + 2];

        for (ArrayList<Integer> op : arr) {
            int L = op.get(0);
            int R = op.get(1);
            diff[L] += 1;
            diff[R + 1] -= 1;
        }

        int max = 0;
        int running = 0;
        for (int i = 1; i <= n; i++) {
            running += diff[i];
            if (running > max) max = running;
        }

        return max;
    }
};

```

---

# Example walkthrough:

**N** = 5, **operations:** (1,5), (2,4)
**diff after ops:** [0, +1, +1, 0, 0, −1, −1]
**Prefix:** [1,2,2,2,1] → max = 2
