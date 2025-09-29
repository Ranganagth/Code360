# Intuition

We want to **minimize the sum of absolute differences** when pairing elements of `ARR1` and `ARR2`.

Key fact:
If we sort both arrays in non-decreasing order and pair them **index by index**, this will minimize the sum.

Why?

* Consider two pairs `(a, c)` and `(b, d)` where `a < b` and `c < d`.
* If we mismatch them as `(a, d)` and `(b, c)`, the sum of absolute differences will always be larger due to the rearrangement inequality.

This is a well-known greedy principle: **sorted pairing minimizes the total absolute difference.**

---

# Approach

1. Sort both arrays.
2. Pair elements at the same index.
3. Sum up `|arr1[i] - arr2[i]|`.

---

# Complexity

* Sorting: `O(N log N)`
* Summation: `O(N)`
* Total: `O(N log N)` per test case
  Works fine since `N ≤ 5000`.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int minimumSum(int[] arr1, int[] arr2, int n) {
        Arrays.sort(arr1);
        Arrays.sort(arr2);

        int total = 0;
        for (int i = 0; i < n; i++) {
            total += Math.abs(arr1[i] - arr2[i]);
        }

        return total;
    }
}
```

---

## Example Walkthrough

Input:
`arr1 = [10, 24, 5, 90, 4]`
`arr2 = [14, 2, 32, 5, 6]`

Step 1: Sort both arrays
`arr1 = [4, 5, 10, 24, 90]`
`arr2 = [2, 5, 6, 14, 32]`

Step 2: Pair and sum differences

* |4 - 2| = 2
* |5 - 5| = 0
* |10 - 6| = 4
* |24 - 14| = 10
* |90 - 32| = 58

Total = 74 

---