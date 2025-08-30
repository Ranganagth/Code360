# Intuition

We are asked to find the **position of the only set bit** (the only `1` in binary representation).

1. If the number has exactly **1 set bit**, then:

   * The number must be a **power of 2** (`n & (n-1) == 0` and `n != 0`).
2. If not, return `-1`.
3. If it is a power of 2:

   * The position of the set bit can be found by **counting trailing zeros + 1**.
   * Example:

     * `N=4` → binary `100` → set bit at position `3`.
     * `N=8` → binary `1000` → set bit at position `4`.

---

# Approach

1. Check if `n == 0` → return `-1`.
2. Check if `n` is a power of 2 using `(n & (n-1)) != 0`:

   * If true, return `-1`.
3. Otherwise, count position:

   * Keep shifting right until `n > 0`.
   * Increment position counter.
   * The first time we hit a `1`, that’s the answer.

---
# Complexity

* Time: `O(log n)` (shifting until bit found).
* Space: `O(1)`.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int findSetBit(int n) {
        if (n == 0) return -1;  // no set bits

        // Check if more than one set bit
        if ((n & (n - 1)) != 0) return -1;

        // Count position of the only set bit
        int pos = 1;
        while ((n & 1) == 0) {
            n >>= 1;
            pos++;
        }
        return pos;
    }
}
```

---

## **Example Walkthrough**

Input: `n = 8`

* Binary: `1000`
* Check: `8 & 7 = 0` → power of 2 → only one set bit.
* Count shifts:

  * `1000` → not `1`, shift → pos=2
  * `0100` → not `1`, shift → pos=3
  * `0010` → not `1`, shift → pos=4
  * `0001` → found set bit → return `4`.

Output: `4`.

---

