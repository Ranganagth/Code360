# Intuition

We need the **largest integer `x`** such that:

```
x * x ≤ A
```

The naive approach would be to start from 0 and increment `x` until `x² > A`.
But that’s **O(√A)** — too slow for large inputs (up to 10⁵).

We can instead use **Binary Search**, because:

* As `x` increases, `x²` also increases **monotonically**.
* So, we can search for the correct `x` efficiently in **O(log A)** time.

---

# Approach (Binary Search)

1. Initialize:

   ```
   low = 0, high = A, ans = 0
   ```
2. While `low <= high`:

   * Compute `mid = (low + high) / 2`
   * If `mid * mid <= A`,
     → `mid` is a **valid candidate**
     → store `ans = mid`
     → move right to find possibly larger one: `low = mid + 1`
   * Else
     → `mid² > A`, move left: `high = mid - 1`
3. Return `ans`.

---
# Complexity

* **Time:** O(log A)
* **Space:** O(1)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int squareRoot(int a) {
        if (a == 0 || a == 1) return a;

        int low = 0, high = a, ans = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            long sq = (long) mid * mid; // use long to prevent overflow

            if (sq == a) return mid;
            else if (sq < a) {
                ans = mid;       // mid is valid
                low = mid + 1;   // try for a larger number
            } else {
                high = mid - 1;  // try for smaller
            }
        }
        return ans;
    }
}
```

---

## Example Walkthrough

### Example: A = 8

```
low = 0, high = 8
mid = 4 → 4² = 16 > 8 → high = 3
mid = 1 → 1² = 1 ≤ 8 → ans = 1 → low = 2
mid = 2 → 2² = 4 ≤ 8 → ans = 2 → low = 3
mid = 3 → 3² = 9 > 8 → high = 2
```

Loop ends → **ans = 2**

---

### Example: A = 9

```
low = 0, high = 9
mid = 4 → 4² = 16 > 9 → high = 3
mid = 1 → 1² = 1 ≤ 9 → ans = 1 → low = 2
mid = 2 → 2² = 4 ≤ 9 → ans = 2 → low = 3
mid = 3 → 3² = 9 ≤ 9 → ans = 3 → low = 4
```

Loop ends → **ans = 3**

---

**Input

```
2
8
9
```

**Output**

```
2
3
```

---

