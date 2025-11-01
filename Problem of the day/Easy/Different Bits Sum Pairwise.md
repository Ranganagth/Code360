# Intuition

We are asked to find

$$\sum_{i=0}^{n-1} \sum_{j=0}^{n-1} f(arr[i], arr[j])$$

where ( f(x, y) ) = number of differing bits between x and y.

Now, notice that we don’t actually need to compare **every pair** — that would be (O(N^2)), which is too slow for (N = 10^4).

So instead, we can **count the contribution of each bit position independently**.


## Key Observation

For each bit position `b` (from 0 to 31 for integers < 10⁹):

* Let `countOn` = number of elements in `arr` that have the **b-th bit = 1**
* Let `countOff` = number of elements that have the **b-th bit = 0**

Now, any pair `(i, j)` where one number has this bit set and the other doesn’t contributes **1 difference** at this bit position.

* Number of such unordered pairs = `countOn * countOff`
* But since we’re counting **ordered pairs (i, j)**, both `(i, j)` and `(j, i)` are valid and contribute -
  so contribution = `2 * countOn * countOff`

Hence, for all bits:

$$\text{answer} = \sum_{b=0}^{31} 2 * countOn * countOff$$

Finally, take modulo (10^9 + 7).

---

# Approach

1. For each bit position `b` from 0 to 31:

   * Count how many numbers have the `b`-th bit set (`countOn`).
   * Compute `countOff = n - countOn`.
   * Add `(2 * countOn * countOff)` to answer.
2. Take modulo (10^9 + 7).

---

# Complexity Analysis

* **Time Complexity:** (O(32 * N)) → (O(N))
* **Space Complexity:** (O(1))

---

## Example Walkthroug

**Example:**
`arr = [1, 2]`

Binary:

```
1 = 0001
2 = 0010
```

* Bit 0: one 1 (from 1), one 0 (from 2) → contribution = 2 * 1 * 1 = 2
* Bit 1: one 1 (from 2), one 0 (from 1) → contribution = 2 * 1 * 1 = 2
* Other bits contribute 0.

Total = 4 → matches output.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int differentBitsSumPairwise(ArrayList<Integer> arr, int n) {
        long MOD = 1000000007L;
        long ans = 0;

        for (int bit = 0; bit < 32; bit++) {
            long countOn = 0;
            for (int num : arr) {
                if ((num & (1 << bit)) != 0) {
                    countOn++;
                }
            }

            long countOff = n - countOn;
            ans = (ans + (2L * countOn * countOff) % MOD) % MOD;
        }

        return (int) ans;
    }
};

```

---

## Example Verification

Input:

```
2
2
1 2
2
6 6
```

Output:

```
4
0
```

Matches perfectly.

---
