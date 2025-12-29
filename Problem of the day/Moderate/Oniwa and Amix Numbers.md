# Intuition

You want $((A \text{ OR } B) = C)$. OR works *bit-by-bit*. So handle each bit independently and count how many flips are needed to fix that bit.

---

# Approach

Scan bits from 0 to 31.

Let $(a_i, b_i, c_i)$ be the i-th bits.
* If $(c_i = 1)$: at least one of $(a_i)$ or $(b_i)$ must be 1.
  * If both are 0 → flip exactly one (cost 1).
    
* If $(c_i = 0)$: both $(a_i)$ and $(b_i)$ must be 0.
  * Flip every bit among $(a_i, b_i)$ that equals 1 (cost = number of 1s there).

Accumulate the flips.

---

# Complexity

* **Time:** (O(1)) — constant 32 iterations
* **Space:** (O(1))

---

# code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int minimumFlips(int a, int b, int c) {
        int flips = 0;

        for (int i = 0; i < 32; i++) {
            int bitA = (a >> i) & 1;
            int bitB = (b >> i) & 1;
            int bitC = (c >> i) & 1;

            if (bitC == 1) {
                if (bitA == 0 && bitB == 0)
                    flips += 1;         // need one of them to become 1
            } else { // bitC == 0
                if (bitA == 1) flips += 1;
                if (bitB == 1) flips += 1;
            }
        }
        return flips;
    }
};

```

---

# Walkthrough example

**Input:** A = 2 (010), **B** = 4 (100), **C** = 5 (101)

**Bit by bit (from LSB):**

* bit0: A=0, B=0, C=1 → need one 1 → flips = 1
* bit1: A=1, B=0, C=0 → both must be 0 → flip A → flips = 2
* bit2: A=0, B=1, C=1 → already satisfies

**Total flips** = 2

----