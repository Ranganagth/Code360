# Intuition

The key observation is:
To group all `1`s together, we essentially need to find a contiguous block in the array of length equal to the total count of `1`s, such that the number of `0`s inside this block is minimized.

Why?
Because every `0` inside this block must be swapped with a `1` outside it to make all `1`s contiguous. Minimizing `0`s inside that block gives us the minimum swaps needed.

---

# Approach

1. **Count the total number of 1s (`totalOnes`)** in the array.

   * If `totalOnes == 0`, return `-1` (no `1`s to group).
   * If `totalOnes == n`, return `0` (all `1`s already together).

2. **Use a sliding window of size `totalOnes`**:

   * Count how many `0`s are in the current window.
   * Slide the window across the array, updating the count of `0`s in `O(1)` time.
   * Keep track of the **minimum number of `0`s** found in any window.

3. The **minimum number of `0`s** in such a window is the answer — the minimum swaps required.

---

# Complexity

* **Time complexity:** `O(n)`
  We traverse the array at most twice — once to count `1`s and once for the sliding window.

* **Space complexity:** `O(1)`
  We only store counters and a few variables.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int groupAllOneTogether(int arr[], int n) {
        // Step 1: Count total number of 1's
        int totalOnes = 0;
        for (int num : arr) {
            if (num == 1) totalOnes++;
        }

        // Edge cases
        if (totalOnes == 0) return -1; // No 1's
        if (totalOnes == n) return 0;  // All 1's already together

        // Step 2: Sliding window of size 'totalOnes'
        int zeroCount = 0;
        for (int i = 0; i < totalOnes; i++) {
            if (arr[i] == 0) zeroCount++;
        }

        int minSwaps = zeroCount;

        // Step 3: Slide the window
        for (int i = totalOnes; i < n; i++) {
            if (arr[i - totalOnes] == 0) zeroCount--; // Element going out
            if (arr[i] == 0) zeroCount++; // Element coming in
            minSwaps = Math.min(minSwaps, zeroCount);
        }

        return minSwaps;
    }

    // Driver code for testing
    public static void main(String[] args) {
        int[] arr1 = {1, 0, 1, 0, 1};
        int[] arr2 = {0, 0, 0, 0};
        int[] arr3 = {1, 1, 0, 0, 1, 1};

        System.out.println(groupAllOneTogether(arr1, arr1.length)); // Output: 1
        System.out.println(groupAllOneTogether(arr2, arr2.length)); // Output: -1
        System.out.println(groupAllOneTogether(arr3, arr3.length)); // Output: 2
    }
}
```

---
