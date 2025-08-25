# Intuition

We’re asked to repeatedly:

1. **Query substring \[l, r] interpreted as binary → mod 3.**
2. **Flip a bit at index idx.**

Naively:

* Extract substring, convert to integer, then mod 3.
* But substring length ≤ 3000 and Q ≤ 3000 → worst case = 9e6 operations → feasible in 1s, but risky in Java if we use big integers.

Observation:

* Instead of converting substring into a number, we can directly compute modulo 3 using **modular arithmetic on binary strings**.

---

# Approach

### Key idea: Modulo properties of binary strings

A binary number can be expressed as:

$$
\text{val}[l, r] = \sum_{i=l}^r S[i] \cdot 2^{r-i}
$$

We only care about modulo 3.

Notice:

* Powers of 2 modulo 3 cycle:

  * 2^0 % 3 = 1
  * 2^1 % 3 = 2
  * 2^2 % 3 = 1
  * 2^3 % 3 = 2 …
* So pattern = \[1, 2, 1, 2, …]

Thus:

$$
\text{val}[l, r] \% 3 = \sum_{i=l}^r S[i] \cdot (2^{r-i} \% 3) \% 3
$$

### Implementation plan:

1. **Precompute powers of 2 modulo 3 up to N** (cycle length = 2).
2. For query type-1:

   * Iterate i = l…r, accumulate `(s[i] - '0') * pow2[r-i] % 3`.
3. For query type-2 (flip):

   * Toggle character at `s[idx]`.

---

# Complexity

* Each type-1 query = O(length of substring) = O(N).
* Each type-2 query = O(1).
* With N, Q ≤ 3000 → worst case ≈ 9e6 operations, which is acceptable in 1 sec.

---

# Code

```java
import java.util.*;

public class Solution {
    public static class query {
        int type;
        int l, r;
        int idx;
    }

    public static int[] binaryFlip(int n, String s, int q, query[] queries) {
        char[] arr = s.toCharArray();
        List<Integer> result = new ArrayList<>();

        // Precompute 2^k % 3 up to n
        int[] pow2mod3 = new int[n];
        pow2mod3[0] = 1 % 3;
        for (int i = 1; i < n; i++) {
            pow2mod3[i] = (pow2mod3[i - 1] * 2) % 3;
        }

        for (int i = 0; i < q; i++) {
            if (queries[i].type == 1) {
                int l = queries[i].l;
                int r = queries[i].r;
                int mod = 0;

                // Compute substring value % 3
                for (int j = l; j <= r; j++) {
                    if (arr[j] == '1') {
                        int powerIndex = r - j;
                        mod = (mod + pow2mod3[powerIndex]) % 3;
                    }
                }
                result.add(mod);

            } else { // type == 2
                int idx = queries[i].idx;
                arr[idx] = (arr[idx] == '0') ? '1' : '0';
            }
        }

        // Convert result to int[]
        int[] ans = new int[result.size()];
        for (int i = 0; i < result.size(); i++) ans[i] = result.get(i);
        return ans;
    }
}
```

---

# Example Walkthrough

Input:

```
N = 5
S = "10010"
Q = 5
Queries:
1 0 4
2 1
1 0 3
2 3
1 1 4
```

Steps:

* Query1: substring "10010" → value = 18 → 18%3 = 0.
* Query2: flip index 1 → S = "11010".
* Query3: substring "1101" → 13 → 13%3 = 1.
* Query4: flip index 3 → S = "11000".
* Query5: substring "1000" → 8 → 8%3 = 2.

Output:

```
0
1
2
```

---
