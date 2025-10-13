# Intuition

We want to count **subarrays with exactly K distinct integers**.

A **subarray** is contiguous, so we can use the **two-pointer sliding window technique** to track the number of distinct elements dynamically as we expand or shrink the window.

However, directly counting “exactly K distinct” subarrays is tricky.
A smarter way is to use this relationship:

> **Count of subarrays with exactly K distinct =
> Count of subarrays with at most K distinct − Count of subarrays with at most (K−1) distinct**

So, our job reduces to efficiently counting **subarrays with at most K distinct elements**.

---

# Approach

**Step 1:** Helper Function

Create a function `countAtMostKDistinct(int[] arr, int n, int k)` that returns the **number of subarrays with at most K distinct elements**.

**Logic for the helper function:**

* Maintain a window `[left, right]` and a HashMap `freq` to count element frequencies.
* Expand `right` one by one:

  * Add `arr[right]` to the map.
  * If distinct elements > K, shrink the window from the left until it becomes valid again.
* At each step, all subarrays ending at `right` and starting anywhere between `left` and `right` are valid, contributing `(right - left + 1)` subarrays.

**Step 2:** Main Function

Return:

```java
countAtMostKDistinct(arr, n, k) - countAtMostKDistinct(arr, n, k - 1)
```

---

# Complexity Analysis

* **Time:** `O(N)` — Each element is added and removed from the hashmap at most once.
* **Space:** `O(K)` — For the hashmap storing counts of up to K distinct elements.

---

# Code

```java
import java.util.*;

public class Solution {

    // Helper function: count of subarrays with at most K distinct elements
    private static int countAtMostKDistinct(int[] arr, int n, int k) {
        if (k == 0) return 0;
        Map<Integer, Integer> freq = new HashMap<>();
        int left = 0, count = 0;

        for (int right = 0; right < n; right++) {
            freq.put(arr[right], freq.getOrDefault(arr[right], 0) + 1);

            // If more than k distinct, shrink window
            while (freq.size() > k) {
                freq.put(arr[left], freq.get(arr[left]) - 1);
                if (freq.get(arr[left]) == 0)
                    freq.remove(arr[left]);
                left++;
            }

            // All subarrays ending at right
            count += (right - left + 1);
        }

        return count;
    }

    public static int kDistinctSubarrays(int[] arr, int n, int k) {
        return countAtMostKDistinct(arr, n, k) - countAtMostKDistinct(arr, n, k - 1);
    }

    // Example test
    public static void main(String[] args) {
        int[] arr1 = {1, 1, 2, 3};
        System.out.println(kDistinctSubarrays(arr1, 4, 2)); // Output: 3

        int[] arr2 = {2, 1, 3, 2, 4};
        System.out.println(kDistinctSubarrays(arr2, 5, 2)); // Output: 9
    }
};

```

---

## Example Walkthrough

**Input:**

```
arr = [1, 1, 2, 3], k = 2
```

We’ll find:

* `atMost(2)` → 9 subarrays
* `atMost(1)` → 6 subarrays
  → `exactly(2) = 9 - 6 = 3`

Which matches the example.

---
### Summary

1. Use the formula:
   `exactly(K) = atMost(K) - atMost(K-1)`
2. Maintain a sliding window to count `atMost(K)` efficiently using a frequency map.
3. Complexity: **O(N)** time, **O(K)** space — suitable for `N ≤ 10^5`.

