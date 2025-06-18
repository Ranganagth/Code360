# Intuition

We want to **maximize the distance** between two indices `i` and `j` such that `arr[j] > arr[i]`. A brute-force solution would check all `i` and `j` pairs which is **O(N²)** — not acceptable for `N ≤ 10^6`.

We need a more optimal approach using preprocessing + two-pointer technique.

---

# Approach

1. **Create leftMin\[]**: `leftMin[i]` stores the minimum value from index `0` to `i`.
2. **Create rightMax\[]**: `rightMax[j]` stores the maximum value from index `j` to `n-1`.
3. Use two pointers `i` and `j` to traverse both arrays:

   * If `leftMin[i] < rightMax[j]`, update `maxDiff = max(maxDiff, j - i)` and move `j` forward.
   * Else, move `i` forward.

This approach is based on the fact that the condition `arr[j] > arr[i]` can be derived from comparing `leftMin[i]` and `rightMax[j]`.

---

# Complexity

* **Time complexity:** $O(n)$
* **Space complexity:** $O(n)$

---

# Code

```java
public class Solution {
    public static int maxIndexDiff(int[] arr, int n) {
        int[] leftMin = new int[n];
        int[] rightMax = new int[n];

        leftMin[0] = arr[0];
        for (int i = 1; i < n; i++) {
            leftMin[i] = Math.min(arr[i], leftMin[i - 1]);
        }

        rightMax[n - 1] = arr[n - 1];
        for (int j = n - 2; j >= 0; j--) {
            rightMax[j] = Math.max(arr[j], rightMax[j + 1]);
        }

        int i = 0, j = 0, maxDiff = -1;

        while (i < n && j < n) {
            if (leftMin[i] < rightMax[j]) {
                maxDiff = Math.max(maxDiff, j - i);
                j++;
            } else {
                i++;
            }
        }

        return maxDiff;
    }
}
```

---
