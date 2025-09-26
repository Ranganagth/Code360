# Intuition

We want to place `k` cows in the stalls so that the **minimum distance between any two cows** is maximized.

* If the minimum distance is too large, it may be **impossible** to place all cows.
* If the minimum distance is small, it’s always **possible**.

This makes it a **search problem**:

* The answer lies between **1** and `max(stalls) - min(stalls)`.
* We can binary search this distance.

---

# Approach

1. **Sort stalls** (positions can be unsorted).
2. Use **binary search** on the answer:

   * `low = 1` (minimum possible distance).
   * `high = stalls[n-1] - stalls[0]` (maximum possible distance).
3. For each `mid = (low + high) / 2`, check if we can place `k` cows with at least `mid` distance apart.

   * Place the first cow at the first stall.
   * Greedily place next cows in the next stall that is at least `mid` away.
   * If we can place all `k` cows → feasible.
4. If feasible → try bigger distance (`low = mid + 1`).
   Else → try smaller distance (`high = mid - 1`).
5. Return `high` (largest feasible minimum distance).

---

# Complexity

* Sorting: **O(n log n)**
* Binary search range: **O(log(max(stalls) - min(stalls))) ≈ O(log(1e9))**
* Feasibility check: **O(n)**

So total: **O(n log n + n log(1e9)) ≈ O(n log n)**, which meets the requirement.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int aggressiveCows(int[] stalls, int k) {
        Arrays.sort(stalls);
        int n = stalls.length;
        
        int low = 1; 
        int high = stalls[n - 1] - stalls[0];
        int ans = 0;
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (canPlace(stalls, k, mid)) {
                ans = mid;       // feasible, try bigger distance
                low = mid + 1;
            } else {
                high = mid - 1;  // not feasible, try smaller distance
            }
        }
        
        return ans;
    }
    
    private static boolean canPlace(int[] stalls, int k, int dist) {
        int count = 1; // first cow placed
        int lastPos = stalls[0];
        
        for (int i = 1; i < stalls.length; i++) {
            if (stalls[i] - lastPos >= dist) {
                count++;
                lastPos = stalls[i];
                if (count >= k) return true;
            }
        }
        return false;
    }
}
```

---

## Example Walkthrough

Input:

```
n = 6, k = 4  
stalls = [0, 3, 4, 7, 10, 9]
```

Sorted: `[0, 3, 4, 7, 9, 10]`

* low = 1, high = 10
* mid = 5 → can we place 4 cows?

  * Place at 0, next at ≥5 (7), next at ≥12 (no). Only 2 cows placed → not feasible.
* Reduce → high = 4.
* mid = 2 → feasible (0, 3, 7, 10).
* Try bigger → low = 3.
* mid = 3 → feasible (0, 3, 7, 10).
* Try bigger → low = 4.
* mid = 4 → feasible (0, 4, 9). Only 3 cows, not enough.
* high = 3.

Final answer = **3**.

---
