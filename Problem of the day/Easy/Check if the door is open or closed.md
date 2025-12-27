# Intuition

Door *k* is toggled once for every person whose ID divides *k*. That means door *k* is toggled exactly as many times as the number of divisors of *k*.
A door ends **open** only if it is toggled an **odd** number of times.
Only **perfect squares** have an odd number of divisors.
Therefore, every door whose index is a perfect square stays open; all others stay closed.

---

# Approach

Iterate from 1 to N.
If the index is a perfect square, append `'1'`. Otherwise append `'0'`.
We check perfect square by computing `i * i <= n` and marking those doors.

---

# Complexity

- **Time:** O(N)
- **Space:** O(1) extra (output string costs O(N))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String doorStatus(int n) {
        StringBuilder sb = new StringBuilder();
        // mark perfect squares as open
        int sq = 1;
        int k = 1;
        for (int i = 1; i <= n; i++) {
            if (i == sq) {
                sb.append('1');
                k++;
                sq = k * k;
            } else {
                sb.append('0');
            }
        }
        return sb.toString();
    }
};

```

---

# Example walkthrough 

**(N = 8)**

Perfect squares ≤ 8: 1, 4.

**Doors:**
1 → open
2 → closed
3 → closed
4 → open
5 → closed
6 → closed
7 → closed
8 → closed

**Result:** `10010000`

---