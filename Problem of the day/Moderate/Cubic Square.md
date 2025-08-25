# Intuition

We need to compute:

$$
A^B \bmod M
$$

* **A** and **M** are given in base 10.
* **B** is given as a string in **base 3**.

The straightforward way would be to **convert B (base 3) → decimal**, then compute `pow(A, B) % M`.
But there are two issues:

1. `B` can be very large (up to 10^5 digits if stored as a string), so direct conversion may overflow.
2. Direct exponentiation is infeasible for large `B`.

We need **modular exponentiation** and a method to handle large exponents represented in base 3.

---

# Approach

1. **Modular Exponentiation with base conversion**

   * Instead of converting `B` fully into decimal, we process it digit by digit in base 3.
   * Suppose `B = b_k b_{k-1} ... b_0` in base 3.
   * That means:

     $$
     B = \sum_{i=0}^k b_i \cdot 3^i
     $$
   * Then:

     $$
     A^B \bmod M = \prod_{i=0}^k \big(A^{3^i}\big)^{b_i} \bmod M
     $$

2. **Efficient Calculation**

   * Start from left to right over digits of B.
   * Maintain a result `res = 1`.
   * For each digit `d` in `B`:

     * First: raise `res` to the 3rd power (because we shift base by 3).
     * Then: multiply by `A^d` (since digit can be 0, 1, or 2).
     * Always apply modulo `M` to keep values small.

This avoids converting `B` into an integer and uses only modular arithmetic.

---

# Complexity

* **Time complexity:** `O(len(B) * log(M))`

  * Each step involves at most a few modular exponentiations (`powMod` up to exponent 2).
* **Space complexity:** `O(1)` extra space (besides input).

---

# Code

```java
import java.util.*;

public class Solution {

    // Helper: fast modular exponentiation
    private static long modPow(long base, long exp, long mod) {
        long result = 1;
        base %= mod;
        while (exp > 0) {
            if ((exp & 1) == 1) {
                result = (result * base) % mod;
            }
            base = (base * base) % mod;
            exp >>= 1;
        }
        return result;
    }

    public static int cubicSquare(int m, int a, String b) {
        long res = 1;
        long base = a % m;

        for (int i = 0; i < b.length(); i++) {
            int digit = b.charAt(i) - '0'; // digit in base 3

            // Step 1: res = (res^3) % m
            res = modPow(res, 3, m);

            // Step 2: multiply by (a^digit) % m
            if (digit > 0) {
                res = (res * modPow(base, digit, m)) % m;
            }
        }
        return (int) res;
    }
}
```

---

# Example Walkthrough

Input:

```
A = 2, B = "11" (base 3), M = 6
```

1. Interpret B: `"11"` in base 3 = decimal 4.
   Expected: $2^4 \mod 6 = 16 \mod 6 = 4$.

2. Processing string `"11"`:

   * Start: `res = 1`.
   * First digit `'1'`:

     * `res = (1^3) % 6 = 1`.
     * Multiply by `2^1 % 6 = 2`. → `res = 2`.
   * Second digit `'1'`:

     * `res = (2^3) % 6 = 8 % 6 = 2`.
     * Multiply by `2^1 % 6 = 2`. → `res = 4`.

Final result = `4`.

---
