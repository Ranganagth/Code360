# Intuition

To find the subarray with the **maximum sum**, we want to consider contiguous elements and keep track of the maximum sum encountered so far. If at any point the running sum becomes negative, we reset it to 0 (since an empty subarray is allowed and has sum 0).

---

# Approach (Kadane’s Algorithm)

1. Initialize:
   - `maxSum = 0` (to allow empty subarray result)
   - `currentSum = 0`
2. Traverse the array:
   - For each element `arr[i]`, do:
     - `currentSum += arr[i]`
     - Update `maxSum = max(maxSum, currentSum)`
     - If `currentSum < 0`, reset `currentSum = 0`
3. Return `maxSum`

---

# Complexity

- **Time complexity:** `O(n)` — Single pass through array
- **Space complexity:** `O(1)` — Constant space used

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static long maxSubarraySum(int[] arr, int n) {
        long maxSum = 0;
        long currentSum = 0;

        for (int i = 0; i < n; i++) {
            currentSum += arr[i];
            if (currentSum > maxSum) {
                maxSum = currentSum;
            }
            if (currentSum < 0) {
                currentSum = 0;
            }
        }

        return maxSum;
    }
}
```

---

### Example

**Input:**
```
9
1 2 7 -4 3 2 -10 9 1
```

**Output:**
```
11
```

**Explanation:** The subarray `[1, 2, 7, -4, 3, 2]` gives the maximum sum of `11`.
