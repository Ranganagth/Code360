# Intuition

For modulo with a prime `P`:

* If `n ≥ p`, then `n!` contains `p` as a factor ⇒ `n! % p = 0`.
* If `n < p`, direct multiplication is impossible when `n` is large.
  Use **Wilson’s Theorem**:
  `(p − 1)! ≡ −1 (mod p)`.

Rewriting:

```
n! ≡ (p−1)! / [(n+1)(n+2)…(p−1)] (mod p)
```

Division modulo a prime becomes multiplication with modular inverses.

---

# Approach

1. If `n ≥ p`, return `0`.
2. Otherwise:

   * Start with `res = p − 1` (since `(p−1)! ≡ −1 ≡ p−1 mod p`)
   * For `i = n+1` to `p−1`:

     * Multiply `res` with modular inverse of `i` modulo `p`
3. Return `res`.

Modular inverse is computed using Fermat’s Little Theorem:

```
a⁻¹ ≡ a^(p−2) (mod p)
```

---

# Complexity

* **Time:** `O(|p − n| · log p)`
  (`|p − n| ≤ 1000` by constraint)
* **Space:** `O(1)`

---

# Code

```java
public class Solution {

    public static int boringFactorials(int n, int p) {
        if (n >= p) return 0;

        long res = p - 1;

        for (int i = n + 1; i <= p - 1; i++) {
            res = (res * modInverse(i, p)) % p;
        }

        return (int) res;
    }

    private static long modInverse(long a, long p) {
        return modPow(a, p - 2, p);
    }

    private static long modPow(long base, long exp, long mod) {
        long result = 1;
        base %= mod;

        while (exp > 0) {
            if ((exp & 1) == 1) result = (result * base) % mod;
            base = (base * base) % mod;
            exp >>= 1;
        }

        return result;
    }
};

```

---

# Example Walkthrough

**Input:**

```
n = 4, p = 5
```

* `n < p`, proceed.
* `(p−1)! mod p = 4`
* No division needed (`n+1 = p`)
* Result = `4`

**Output:**

```
4
```

**Another:**

```
n = 10, p = 7
```

* `n ≥ p`
* Factor `7` exists in `10!`
* Result = `0`

---
