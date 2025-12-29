# Intuition

Build the number digit-by-digit from left to right. To keep the number **minimal**, each position must take the **smallest digit possible** while still allowing the remaining digits to reach the remaining sum.

Feasibility rules
If `k > 9*n`, impossible — even all 9s can’t reach the sum.
If `k == 0` and `n > 1`, impossible — you’d get a leading zero.
If impossible → return `-1`.

---

# Approach

1. Handle the impossible cases above.
2. For each position `i` from `0 … n-1`
   • Let `remaining = n - i - 1`.
   • Try digits from `low` to `9`, where
   – `low = 1` for the first digit, otherwise `0`.
   • Pick the **smallest digit `d`** such that

   ```
   0 <= k - d <= 9 * remaining
   ```

   • Append `d`, subtract it from `k`.
3. The built string is the smallest valid number.

---

# Complexity

- **Time:** `O(n)`
- **Space:** `O(n)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String smallestNumber(Integer n, Integer k) {

        if (k > 9 * n) return "-1";
        if (k == 0) return (n == 1) ? "0" : "-1";

        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < n; i++) {
            int remaining = n - i - 1;

            int low = (i == 0 ? (n == 1 ? 0 : 1) : 0);

            for (int d = low; d <= 9; d++) {
                int rest = k - d;
                if (rest >= 0 && rest <= 9 * remaining) {
                    sb.append(d);
                    k -= d;
                    break;
                }
            }
        }

        return sb.toString();
    }
};

```

---

# Example walkthrough

**N** = 4, **K** = 5

Position 0: try 1 → remaining sum = 4, possible (≤ 27). take 1.  k=4
Position 1: try 0 → remaining sum = 4, possible (≤ 18). take 0.  k=4
Position 2: try 0 → remaining sum = 4, possible (≤ 9).  take 0.  k=4
Position 3: only digit must be 4.

Result: **1004**
