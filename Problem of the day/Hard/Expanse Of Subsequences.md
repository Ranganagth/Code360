# Intuition

Each element in the array can act as:
* **Maximum** of some subsequences
* **Minimum** of some subsequences

Let’s denote:
* `arr[i]` appears as **maximum** in `2^i` subsequences.
* `arr[i]` appears as **minimum** in `2^(N - 1 - i)` subsequences.

So, for each `arr[i]`, the **total contribution** to the expanse sum is:

```
(arr[i] * 2^i) - (arr[i] * 2^(N - 1 - i))
```

We compute this for all `i` and take the sum modulo `10^9 + 7`.

---

# Approach

1. Sort the array.
2. Precompute powers of 2 up to `N` modulo `10^9 + 7`.
3. For each index `i` in `arr`, calculate:

   ```
   contribution = (arr[i] * pow2[i]) - (arr[i] * pow2[N - 1 - i])
   ```
4. Add all contributions together.

---

# Complexity Analysis

* **Time Complexity:** `O(N log N)` (for sorting) + `O(N)` (for contribution sum)
* **Space Complexity:** `O(N)` for precomputed powers

---

# Code

```java
import java.util.*;

public class Solution {

    static final int MOD = 1_000_000_007;

    public static int expanseOfSubsequences(int[] arr) {
        int n = arr.length;
        Arrays.sort(arr);

        // Precompute powers of 2 modulo MOD
        long[] pow2 = new long[n];
        pow2[0] = 1;
        for (int i = 1; i < n; i++) {
            pow2[i] = (pow2[i - 1] * 2) % MOD;
        }

        long result = 0;
        for (int i = 0; i < n; i++) {
            long maxContribution = (arr[i] * pow2[i]) % MOD;
            long minContribution = (arr[i] * pow2[n - 1 - i]) % MOD;
            result = (result + maxContribution - minContribution + MOD) % MOD;
        }

        return (int) result;
    }
}
```

---

### **Example Walkthrough**

#### Input:

```text
arr = [2, 1, 3]
```

* Sorted: \[1, 2, 3]
* N = 3
* Powers: \[1, 2, 4]

Calculate contributions:

* For 1 (i=0): max = 1×1, min = 1×4 → contribution = 1 - 4 = -3
* For 2 (i=1): max = 2×2, min = 2×2 → contribution = 4 - 4 = 0
* For 3 (i=2): max = 3×4, min = 3×1 → contribution = 12 - 3 = 9

Total = (-3 + 0 + 9) = 6

Matches expected output.

---
