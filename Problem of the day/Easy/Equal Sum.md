## Understanding the Problem

This is essentially the **Equilibrium Index problem** where we need to find an index `i` such that the sum of elements before `i` equals the sum of elements after `i`.
We’ll solve it efficiently in **O(N)** time and **O(1)** extra space.

---

# Intuition

Alex collects tokens from the **left side (before index i)** and Rome collects tokens from the **right side (after index i)**.
At equilibrium, the sum of Alex’s tokens = sum of Rome’s tokens.
This is possible if at checkpoint `i`:

```
prefixSum (0 to i-1) == totalSum - prefixSum (0 to i) - token[i]
```

---

# Approach

1. Compute the **total sum** of all tokens.
2. Initialize `leftSum = 0`.
3. Traverse the array from left to right:

   * At each index `i`, compute `rightSum = totalSum - leftSum - token[i]`.
   * If `leftSum == rightSum`, return `i` (smallest index is guaranteed since we traverse left to right).
   * Otherwise, update `leftSum += token[i]`.
4. If no equilibrium index is found, return `-1`.

---

# Complexity

* **Time complexity:** `O(N)` (single pass through the array).
* **Space complexity:** `O(1)` (only variables, no extra data structures).

---

# Code

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {

    public static int equalSum(ArrayList<Integer> token) {
        int n = token.size();
        long totalSum = 0;
        for (int val : token) {
            totalSum += val;
        }

        long leftSum = 0;
        for (int i = 0; i < n; i++) {
            long rightSum = totalSum - leftSum - token.get(i);
            if (leftSum == rightSum) {
                return i;
            }
            leftSum += token.get(i);
        }
        return -1;
    }
}
```

---
