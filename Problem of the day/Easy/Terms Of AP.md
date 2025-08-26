# Intuition

We need the first **X terms** of the series:

$$
f(N) = 3N + 2, \quad N = 1, 2, 3, \dots
$$

but we **skip values divisible by 4**.

Example:

* N=1 → 5 (valid)
* N=2 → 8 (skip, divisible by 4)
* N=3 → 11 (valid)
* N=4 → 14 (valid)
* N=5 → 17 (valid)
* N=6 → 20 (skip)
* and so on.

So the task reduces to: keep generating terms of `3N+2`, skip if divisible by 4, until we collect **X terms**.

---

# Approach

1. Start with an empty list/array.
2. Iterate `n` starting from 1, compute `term = 3*n + 2`.
3. If `term % 4 != 0`, add it to the result.
4. Stop when we have collected **x terms**.
5. Return the result array.

This is efficient since each check is O(1).
Worst case: for `x = 1e5`, we may need a bit more than `x` iterations (since \~1/4 terms are skipped), still O(x).

---

# Complexity

* **Time complexity:** O(x) (we generate about 4/3 × x terms max).
* **Space complexity:** O(x) for the result array.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int[] termsOfAP(int x) {
        int[] result = new int[x];
        int count = 0;
        int n = 1;

        while (count < x) {
            int term = 3 * n + 2;
            if (term % 4 != 0) {
                result[count] = term;
                count++;
            }
            n++;
        }

        return result;
    }
}
```

---

# Example Walkthrough

Input: `x = 5`

* N=1 → 5 → valid → \[5]
* N=2 → 8 → skip
* N=3 → 11 → valid → \[5, 11]
* N=4 → 14 → valid → \[5, 11, 14]
* N=5 → 17 → valid → \[5, 11, 14, 17]
* N=6 → 20 → skip
* N=7 → 23 → valid → \[5, 11, 14, 17, 23]

Output: `[5, 11, 14, 17, 23]`.

---
