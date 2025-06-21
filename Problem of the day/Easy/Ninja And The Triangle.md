# Intuition

To form a triangle of height `h`, we need:

```
Total stars = 1 + 2 + 3 + ... + h = h * (h + 1) / 2
```

So, for a given number of stars `n`, we need to **find the maximum `h`** such that:

```
h * (h + 1) / 2 <= n
```

This is a **monotonic condition**, so we can use **binary search** to find the maximum `h` satisfying it.

---

# Approach

1. Use binary search over height `h` in the range `[1, n]`.
2. For a midpoint `mid`, check if `mid * (mid + 1) / 2 <= n`.
   - If true, this `mid` is a candidate — move right to check for higher.
   - Else, move left.
3. The answer is the highest such `mid`.

---

# Complexity

- **Time:** `O(log n)` per test case
- **Space:** `O(1)`

---

# Code

```java
import java.util.*;

public class Solution {
    public static int ninjaAndTriangle(int n) {
        int low = 1, high = n;
        int answer = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            long requiredStars = (long) mid * (mid + 1) / 2;

            if (requiredStars <= n) {
                answer = mid; // valid height
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return answer;
    }
}
```

---

### Example

**Input:**
```
n = 6
```

**Valid triangle heights:**
```
1 + 2 + 3 = 6 → height = 3 is valid
```

So output is:
```
3
```

