# Intuition

Maximizing moves means breaking (N) into the **maximum number of factors > 1**.

Optimal strategy:

* Always divide by **prime factors**
* Each division removes exactly one prime factor

Thus, answer = **total number of prime factors (with multiplicity)** of
$$N = \frac{a!}{b!} = (b+1) \cdot (b+2) \cdots a$$

So the problem reduces to:
Count total prime factors in all numbers from (b+1) to (a)

---

# Approach

1. Precompute smallest prime factor (SPF) for all numbers up to (a) using sieve.
2. For each number (i) from (b+1) to (a):

   * Factorize using SPF
   * Count number of prime factors
3. Sum all counts

---

# Complexity

* **Time complexity:**
  $$O(n \log n)$$

* **Space complexity:**
  $$O(n)$$

---

# Code

```Java
import java.util.*;
import java.io.*;

public class Solution {

    public static int rounds(int a, int b) {

        if (a == b) return 0;

        int[] spf = new int[a + 1];

        // Sieve for smallest prime factor
        for (int i = 2; i <= a; i++) {
            if (spf[i] == 0) {
                for (int j = i; j <= a; j += i) {
                    if (spf[j] == 0) {
                        spf[j] = i;
                    }
                }
            }
        }

        int count = 0;

        for (int i = b + 1; i <= a; i++) {
            int num = i;

            while (num > 1) {
                count++;
                num /= spf[num];
            }
        }

        return count;
    }
}
```

---

# Example Walkthrough

## Example 1: Step by step explanation

Input: a = 3, b = 1

N = 3! / 1! = 6

Prime factorization:
6 = 2 × 3

Moves:

* Divide by 3 → 2
* Divide by 2 → 1

Total moves = 2

---

## Example 2:

Input: a = 6, b = 4

N = 6! / 4! = 5 × 6 = 30

Prime factors:
30 = 2 × 3 × 5

Total moves = 3
