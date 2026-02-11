# Intuition

The array must be split at some index `i` into two **contiguous** non-empty parts:

* Left sum = prefix sum up to `i`
* Right sum = total sum − prefix sum

For every possible split, compute:
$$
|,\text{leftSum} - \text{rightSum},|
= |,2 \cdot \text{prefixSum} - \text{totalSum},|
$$

The minimum across all valid split points gives the result. Only a single linear scan is required.

---

# Approach

1. Compute `totalSum` of the entire array.
2. Traverse the array while maintaining `prefixSum`.
3. For each split index `i` from `0` to `n-2` (ensuring both parts non-empty):

   * compute `diff = abs(2 * prefixSum - totalSum)`
   * update the minimum difference.
4. Return the minimum value.

---

# Complexity

* **Time:** (O(n))
* **Space:** (O(1))

---

# Code

```java
public class Solution {
    public static int minimumDifference(int n, int[] arr) {

        long total = 0;
        for (int v : arr) total += v;

        long prefix = 0;
        long ans = Long.MAX_VALUE;

        // split must leave right side non-empty
        for (int i = 0; i < n - 1; i++) {
            prefix += arr[i];
            long diff = Math.abs(2 * prefix - total);
            if (diff < ans) ans = diff;
        }

        return (int)ans;
    }
};

```

---

# Example walkthrough with explanation

Array: `[6,7,1,8,5]`

Total sum = 27

Splits:

* `[6] | [7,1,8,5]` → |6 − 21| = 15
* `[6,7] | [1,8,5]` → |13 − 14| = 1
* `[6,7,1] | [8,5]` → |14 − 13| = 1
* `[6,7,1,8] | [5]` → |22 − 5| = 17

Minimum = **1**.
