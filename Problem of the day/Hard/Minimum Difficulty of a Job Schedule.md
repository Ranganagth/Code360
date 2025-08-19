# Intuition

We are asked to **split the array into exactly `K` subarrays** and minimize the **sum of maximums of each subarray**.
This is a **dynamic programming partitioning problem**.

* If `K = 1`, the only option is to take the whole array, so the answer is the maximum of the entire array.
* If `K = N`, then each element is its own subarray, so the sum is just the sum of all elements.
* For general `K`, we need to find the best partitioning.

We want to **decide where to cut** the array into `K` parts such that the sum of the maximum values of each part is minimized.

---

# Approach (DP)

We define:

* `dp[i][k]` = minimum possible sum of maximums when dividing the **first `i` elements** into `k` subarrays.

### Recurrence:

To compute `dp[i][k]`:

* Consider the last subarray ending at position `i`.
* Let that last subarray start from index `j+1` to `i` (`1 ≤ j < i`).
* Then the cost is:

  ```
  dp[i][k] = min( dp[j][k-1] + max(arr[j+1...i]) ) for all j < i
  ```
* Base case:

  * `dp[i][1] = max(arr[0...i])` (only one subarray possible).

### Answer:

`dp[n][K]` where `n = arr.length`.

---

# Complexity

* For each state `(i, k)`, we may check all `j < i`.
* Time: `O(N^2 * K)`
* Since `N ≤ 1000`, `K ≤ N`, this is feasible (`≈ 10^9` in worst naive case, but often optimized with max tracking).

We can optimize by maintaining the maximum while iterating backwards instead of recomputing each time.

---

# Code

```java
import java.util.*;

public class Solution {

    public static int subarrayMaximum(int[] arr, int k) {
        int n = arr.length;

        // dp[i][j] = min sum of max when splitting first i elements into j parts
        int[][] dp = new int[n + 1][k + 1];

        // initialize with large value
        for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);

        // base case: dp[i][1] = max of first i elements
        int maxVal = Integer.MIN_VALUE;
        for (int i = 1; i <= n; i++) {
            maxVal = Math.max(maxVal, arr[i - 1]);
            dp[i][1] = maxVal;
        }

        // fill DP table
        for (int parts = 2; parts <= k; parts++) {
            for (int i = parts; i <= n; i++) {
                int currentMax = Integer.MIN_VALUE;
                // try partition point
                for (int j = i; j >= parts; j--) {
                    currentMax = Math.max(currentMax, arr[j - 1]);
                    if (dp[j - 1][parts - 1] != Integer.MAX_VALUE) {
                        dp[i][parts] = Math.min(dp[i][parts],
                                dp[j - 1][parts - 1] + currentMax);
                    }
                }
            }
        }

        return dp[n][k];
    }

    // For local testing
    public static void main(String[] args) {
        int[] arr1 = {3, 4, 6, 7, 2};
        System.out.println(subarrayMaximum(arr1, 3)); // Expected 12

        int[] arr2 = {1, 2, 3};
        System.out.println(subarrayMaximum(arr2, 3)); // Expected 6
    }
}
```

---

## Example Walkthrough

### Input:

```
ARR = [3, 4, 6, 7, 2], K = 3
```

1. Base case (`K=1`):

   * `dp[5][1] = max(3,4,6,7,2) = 7`

2. For `K=2`:

   * Split `[3,4,6,7] | [2]` → 7 + 2 = 9
   * Split `[3,4] | [6,7,2]` → 4 + 7 = 11
     → best = 9

3. For `K=3`:

   * `[3] | [4,6,7] | [2]` → 3 + 7 + 2 = 12
   * `[3,4] | [6] | [7,2]` → 4 + 6 + 7 = 17
     → best = 12

**Answer = 12**

---
