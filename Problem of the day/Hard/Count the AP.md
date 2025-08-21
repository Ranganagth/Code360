# Intuition

An arithmetic subsequence is defined by a constant difference between consecutive terms.

* If we fix the **last element** of the subsequence and its **previous element**, then the common difference is determined.
* For each pair `(i, j)` with `i < j`, we can try to extend subsequences ending at `i` to include `A[j]`.
* This way, we can dynamically build and count subsequences with the same difference.

---

# Approach

1. **DP State**:

   * Let `dp[j]` be a map where `dp[j][d]` = number of arithmetic subsequences ending at index `j` with common difference `d`.
   * Here, `d = A[j] - A[i]`.

2. **Transition**:

   * For each pair `(i, j)` with `i < j`:

     * Compute `diff = A[j] - A[i]`.
     * If there are subsequences ending at `i` with difference `diff`, we can extend them to `j`.
     * Update:

       ```
       dp[j][diff] += dp[i][diff] + 1
       ```

       * `+1` accounts for the new subsequence of length 2 formed by `(A[i], A[j])`.

3. **Counting only subsequences of length ≥ 3**:

   * Note that `dp[i][diff]` already stores subsequences of length ≥ 2.
   * So, when we extend them, only the **existing subsequences (`dp[i][diff]`)** contribute to valid APs (length ≥ 3).
   * Hence we add `dp[i][diff]` to the answer.

4. **Final Answer**:

   * Sum over all valid contributions.

---

# Complexity

* **Time complexity:**

  * We check all pairs `(i, j)` → `O(n^2)`.
  * Each DP update is `O(1)` average with `HashMap`.
  * Total: `O(n^2)`.
* **Space complexity:**

  * We store differences in `HashMap` for each index.
  * Worst case `O(n^2)`.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int countAP(int n, int[] A) {
        // DP array: each index stores a map of difference -> count of subsequences ending here
        List<HashMap<Long, Integer>> dp = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            dp.add(new HashMap<>());
        }

        int ans = 0;

        for (int j = 0; j < n; j++) {
            for (int i = 0; i < j; i++) {
                long diff = (long) A[j] - (long) A[i];

                // Get previous count of subsequences ending at i with this diff
                int cntAtI = dp.get(i).getOrDefault(diff, 0);

                // Add to result only subsequences of length >= 3
                ans += cntAtI;

                // Update dp[j][diff]
                int newCount = dp.get(j).getOrDefault(diff, 0) + cntAtI + 1;
                dp.get(j).put(diff, newCount);
            }
        }
        return ans;
    }

    // Driver for testing
    public static void main(String[] args) {
        int[] arr1 = {1, 2, 2, 3};
        System.out.println(countAP(arr1.length, arr1)); // Expected 2

        int[] arr2 = {8, 9, -1};
        System.out.println(countAP(arr2.length, arr2)); // Expected 0

        int[] arr3 = {1, 2, 2, 3, 4};
        System.out.println(countAP(arr3.length, arr3)); // Expected 6

        int[] arr4 = {2, 2, 2};
        System.out.println(countAP(arr4.length, arr4)); // Expected 1
    }
}
```

---

# Example Walkthrough

**Input**: `A = [1, 2, 2, 3]`

* Subsequences of length ≥ 3 that are APs:

  * `[1, 2, 3]` using indices `[1,2,4]`
  * `[1, 2, 3]` using indices `[1,3,4]`
* Total = **2**.

**Output**: `2` 

---
