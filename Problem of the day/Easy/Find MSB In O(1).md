# Intuition

We’re asked to find the **largest power of 2 ≤ N**.
A **power of 2** in binary looks like:
* `1` (binary: `1`)
* `2` (binary: `10`)
* `4` (binary: `100`)
* `8` (binary: `1000`)
* ...

The **largest power of 2 ≤ N** will simply be the **most significant bit (MSB)** set in `N`.

For example:
* N = `22` (binary: `10110`)
* MSB is the `10000` place → decimal **16**

---

# Approach

We can find the MSB in **O(1)** using bit operations.

### Bit logic:

1. Keep shifting bits right until only one bit remains.
   This means we’ve found the **position** of the MSB.
2. Shift `1` left by that many times to get the actual power of 2.

Example for N = 22:

```
22 → binary 10110  
Shift until 1:  
10110 → 1011 → 101 → 10 → 1 (shift count = 4)  
Answer = 1 << 4 = 16
```

---

# Complexity

* **Time:** O(log N) if using shifting loop, but if using Java’s `Integer.highestOneBit(n)`, it’s O(1) (internally it uses bit tricks).
* **Space:** O(1)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int findMSB(int n) {
        // Constant time using built-in method
        return Integer.highestOneBit(n);
    }
}
```

---

## Example Walkthrough

### Example 1:

**Input:** N = 4
Binary: `100` → already a power of 2
`Integer.highestOneBit(4)` = 4

**Output:** 4

---

### Example 2:

**Input:** N = 22
Binary: `10110`
MSB position → `10000` (16)
`Integer.highestOneBit(22)` = 16

**Output:** 16

---

### Example 3:

**Input:** N = 63
Binary: `111111`
MSB → `100000` (32)
`Integer.highestOneBit(63)` = 32

**Output:** 32

---

