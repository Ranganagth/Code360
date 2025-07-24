# Intuition

We are asked to divide an array into `K` subsets such that each subset has the same sum. This is a variation of the classic **Partition to K Equal Subsets** problem. If the total sum of the array is not divisible by `K`, it's clearly impossible. The main challenge is to assign each element to one of the `K` subsets such that all subsets sum up to the same target.

---

# Approach

1. **Initial Checks**:

   * Calculate total sum of the array.
   * If total sum is not divisible by `K`, return `false`.
   * If any individual element is greater than the target subset sum, return `false`.

2. **Backtracking**:

   * Sort the array in descending order to optimize pruning.
   * Create a `bucket` array of size `K` to track the current sum in each subset.
   * Recursively try to place each element into one of the `K` subsets.
   * Use backtracking to explore possible combinations.
   * If a number doesn’t fit into any bucket, backtrack.

3. **Optimization**:

   * Prune same state branches by skipping if two buckets have the same value (avoid permutation of same state).
   * Sort descending so large numbers are placed early (fail fast).

---

# Complexity

* **Time complexity**: `O(K^N)` in the worst case, but with optimizations and pruning it performs much better.
* **Space complexity**: `O(K + N)` for recursion stack and bucket array.

---

# Code

```java
import java.util.*;

public class Solution {
    public static boolean splitArray(int[] arr, int K) {
        int n = arr.length;
        int totalSum = Arrays.stream(arr).sum();

        if (totalSum % K != 0) return false;
        int target = totalSum / K;

        Arrays.sort(arr);
        int i = n - 1;
        if (arr[i] > target) return false; // optimization

        int[] buckets = new int[K];
        Arrays.fill(buckets, 0);

        return backtrack(arr, i, buckets, target);
    }

    private static boolean backtrack(int[] arr, int index, int[] buckets, int target) {
        if (index < 0) {
            for (int sum : buckets) {
                if (sum != target) return false;
            }
            return true;
        }

        int curr = arr[index];
        for (int i = 0; i < buckets.length; i++) {
            if (buckets[i] + curr <= target) {
                buckets[i] += curr;
                if (backtrack(arr, index - 1, buckets, target)) return true;
                buckets[i] -= curr;
            }

            // Optimization: if current bucket is empty, skip trying the same for others
            if (buckets[i] == 0) break;
        }

        return false;
    }
}
```

---

This implementation should handle all edge cases efficiently within the constraints (`N ≤ 1000`, `K ≤ 20`). 


```java
import java.util.*;

public class Solution {
    static boolean splitArray(int arr[], int K) {
        int totalSum = Arrays.stream(arr).sum();
        if (totalSum % K != 0) return false;

        int target = totalSum / K;
        Arrays.sort(arr);
        int n = arr.length;

        // Optimization: start from large values
        reverse(arr);

        if (arr[0] > target) return false;

        boolean[] used = new boolean[n];
        return canPartition(0, K, 0, target, arr, used);
    }

    private static boolean canPartition(int start, int k, int currentSum, int target,
                                        int[] arr, boolean[] used) {
        if (k == 0) return true;
        if (currentSum == target)
            return canPartition(0, k - 1, 0, target, arr, used);

        for (int i = start; i < arr.length; i++) {
            if (used[i] || currentSum + arr[i] > target)
                continue;

            used[i] = true;
            if (canPartition(i + 1, k, currentSum + arr[i], target, arr, used))
                return true;
            used[i] = false;

            // Important: if currentSum is 0, and it didn’t work — skip future attempts (no duplicate empty buckets)
            if (currentSum == 0) break;
        }

        return false;
    }

    private static void reverse(int[] arr) {
        int i = 0, j = arr.length - 1;
        while (i < j) {
            int temp = arr[i];
            arr[i++] = arr[j];
            arr[j--] = temp;
        }
    }
}

```