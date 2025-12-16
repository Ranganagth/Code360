# Intuition

Each operation removes exactly one element from either end.
After `K` operations, exactly `K` elements are taken in total.
Those `K` elements must consist of:

* `i` elements from the start
* `K - i` elements from the end

for some `i` in `[0, K]`.

Order of picking does not matter. Only the chosen set matters.
So the problem reduces to choosing the best split between prefix and suffix whose sum is maximum.

---

# Approach

1. Precompute prefix sums:

   * `pref[i]` = sum of first `i` elements.
2. Precompute suffix sums:

   * `suff[i]` = sum of last `i` elements.
3. For every `i` from `0` to `K`:

   * Take `i` elements from start → `pref[i]`
   * Take `K - i` elements from end → `suff[K - i]`
   * Compute total = `pref[i] + suff[K - i]`
4. Return the maximum total.


**Correctness Argument**

Every valid sequence of operations removes exactly `K` elements split between the start and end.
For any such sequence, there exists an `i` such that exactly `i` elements are taken from the front and `K - i` from the back.
The algorithm evaluates all such splits and selects the maximum possible sum.
Therefore, the returned score is optimal.

---
# Complexity

**Time Complexity**

* Prefix + suffix computation: `O(N)`
* Checking all splits: `O(K)`
* Total: `O(N)`

**Space Complexity**

* Prefix and suffix arrays: `O(N)`

---

# Code

```java
import java.util.*;

public class Solution {
    public static int maximizeScore(ArrayList<Integer> arr, int n, int k) {
        int[] pref = new int[n + 1];
        int[] suff = new int[n + 1];

        for (int i = 1; i <= n; i++) {
            pref[i] = pref[i - 1] + arr.get(i - 1);
        }

        for (int i = 1; i <= n; i++) {
            suff[i] = suff[i - 1] + arr.get(n - i);
        }

        int maxScore = 0;

        for (int i = 0; i <= k; i++) {
            maxScore = Math.max(maxScore, pref[i] + suff[k - i]);
        }

        return maxScore;
    }
};

```

---

# Example Walkthrough

**Input:**

```
arr = [10, 5, 6, 7, 9], K = 2
```

**Prefix sums:**

```
pref = [0, 10, 15, 21, 28, 37]
```

**Suffix sums:**

```
suff = [0, 9, 16, 22, 27, 37]
```

**Check splits:**

* `i = 0`: `0 + 16 = 16`
* `i = 1`: `10 + 9 = 19`
* `i = 2`: `15 + 0 = 15`

**Maximum** = `19`
