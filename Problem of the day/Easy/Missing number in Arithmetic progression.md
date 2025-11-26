# Intuition

AP has constant difference d. With exactly one missing value, the deviation from the expected AP begins at the first index where arr[i] ≠ arr[0] + i·d. Use binary search to locate that boundary. Everything before boundary is correct; first incorrect index gives the missing number.

---

# Approach

Compute true common difference: d = (arr[n-1] − arr[0]) / n.
Binary search on index mid:
expected = arr[0] + mid·d.
If arr[mid] == expected → missing lies to the right.
Else → missing lies to the left or at mid.
When binary search ends at low, missing = arr[0] + low·d.

---

# Complexity

- **Time:** O(log n)
- **Space:** O(1)

---

# Code

```java
import java.util.*;

public class Solution {
    public static int missingNumber(int[] arr, int n) {
        int d = (arr[n - 1] - arr[0]) / n;

        int low = 0, high = n - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            long expected = (long) arr[0] + (long) mid * d;

            if (arr[mid] == expected) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return arr[0] + low * d;
    }
}
```

---

# Example

arr = [5, 10, 20, 25], n = 4
d = (25 − 5) / 4 = 5
Expected AP: 5,10,15,20,25

**Binary search:**
mid=1 → arr[1]=10, expected=10 → ok → low=2
mid=2 → arr[2]=20, expected=15 → mismatch → high=1
low=2 → missing = 5 + 2·5 = 15
