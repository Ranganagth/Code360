# Intuition

Track one variable that represents current gifts. Reject any demand larger than current gifts. After a valid deduction, if current gifts fall below *k*, reset to *n*. No branching beyond that.

---

# Approach

1. `cur = n`.
2. For each demand `d` in order:
   • If `d > cur`, mark answer as `-1` and continue.
   • Else: `cur -= d`.
   • If `cur < k`, set `cur = n`.
   • Record current `cur`.
3. Return the list.

---

# Complexity

• Time: O(Q)
• Space: O(Q)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int[] giftsLeft(int n, int k, int q, int[] demands) {
        int[] ans = new int[q];
        int cur = n;

        for (int i = 0; i < q; i++) {
            int d = demands[i];

            if (d > cur) {
                ans[i] = -1;
                continue;
            }

            cur -= d;

            if (cur < k) {
                cur = n;
            }

            ans[i] = cur;
        }

        return ans;
    }
};

```

---

# Example walk-through

n = 10, k = 4
demands = [4, 2, 2]

• Start cur = 10
• Demand = 4 → cur = 6 → ≥ k → record 6
• Demand = 2 → cur = 4 → = k → record 4
• Demand = 2 → cur = 2 → < k → refill → cur = 10 → record 10
