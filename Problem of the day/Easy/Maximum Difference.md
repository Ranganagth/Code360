# Intuition

* The maximum difference between two elements in an array is always:
  **max(arr) – min(arr)**
* We don’t need to check every pair; just find the minimum and maximum.
* Once we have the difference, check its parity (even/odd).

---

# Approach

1. Traverse the array once to find:

   * `minVal = minimum element`
   * `maxVal = maximum element`
2. Compute `diff = maxVal - minVal`.
3. If `diff % 2 == 0` → return `"EVEN"` else → `"ODD"`.

---

# Complexity

* **Time Complexity:** `O(N)` (single scan of array).
* **Space Complexity:** `O(1)` (just storing two variables).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static String maximumDifference(int n, int[] arr) {
        int minVal = Integer.MAX_VALUE;
        int maxVal = Integer.MIN_VALUE;
        
        // Find min and max in one pass
        for (int num : arr) {
            if (num < minVal) minVal = num;
            if (num > maxVal) maxVal = num;
        }
        
        int diff = maxVal - minVal;
        
        if (diff % 2 == 0) {
            return "EVEN";
        } else {
            return "ODD";
        }
    }
};

```

---

## Example Walkthrough

Input:

```
arr = [2, 9, 3, 4]
```

* min = 2, max = 9
* maxDiff = 9 - 2 = 7 → odd → `"ODD"`

Input:

```
arr = [1, 1, 1, 1, 1, 1]
```

* min = 1, max = 1
* maxDiff = 0 → even → `"EVEN"`

---
