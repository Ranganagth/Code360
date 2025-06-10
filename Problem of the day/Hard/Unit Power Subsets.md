> Partially Accepted
# Intuition

We are asked to count subsets where the **product of elements is composed of *distinct* primes** (i.e., no prime appears more than once in the product). For example:

* Valid: `3 * 5 = 15` (distinct primes),
* Invalid: `3 * 3 = 9` (prime 3 appears twice).

We need to:

1. Remove numbers with **repeated prime factors**.
2. Use **bitmasking** to represent sets of primes, since all valid numbers must form subsets where the same prime doesn't repeat.

---

# Approach

1. **Preprocessing:**
   * Precompute a list of primes ≤ 30 (as elements are ≤ 30).
   * Map each number \[2–30] to a **bitmask** of its prime factors only if all are distinct (e.g., 6 → 2\*3 → mask: `0011`).

2. **Filtering:**
   * Ignore numbers like `4` (2\*2), `8` (2\*2\*2), `18` (2\*3\*3) as they use repeated primes.

3. **Counting:**
   * Use **Dynamic Programming** with bitmasks (`dp[mask]`) to track combinations of valid subsets.
   * For each valid number's mask, try combining with existing masks (no overlapping primes).

4. **Handling `1`s:**
   * Any subset can include any number of `1`s, as 1 doesn't affect the product.
   * Total subsets = valid subsets × (2^countOf1s)

5. **Modulo:** Use `MOD = 10^9 + 7` as specified.

---

# Complexity

* **Time complexity:**
  $O(N \cdot 2^P)$
  where $P$ is number of distinct primes ≤ 30 (at most 10).

* **Space complexity:**
  $O(2^P)$
  For storing `dp[mask]`.

---

# Code

```java
import java.util.*;

public class Solution {
    static final int MOD = 1_000_000_007;
    static final int[] PRIMES = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    static final int[] primeMask = new int[31];

    static {
        for (int num = 2; num <= 30; num++) {
            int mask = 0;
            int x = num;
            boolean isValid = true;
            for (int i = 0; i < PRIMES.length; i++) {
                int prime = PRIMES[i];
                int count = 0;
                while (x % prime == 0) {
                    x /= prime;
                    count++;
                }
                if (count > 1) {
                    isValid = false;
                    break;
                }
                if (count == 1) {
                    mask |= (1 << i);
                }
            }
            if (x > 1) isValid = false; // if num has a prime factor > 29
            primeMask[num] = isValid ? mask : -1;
        }
    }

    import java.util.* ;
import java.io.*; 
public class Solution {

    static final int MOD = 1_000_000_007;
    static final int[] PRIMES = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29};
    static final int[] primeMask = new int[31];

    static {
        for (int num = 2; num <= 30; num++) {
            int mask = 0;
            int x = num;
            boolean isValid = true;
            for (int i = 0; i < PRIMES.length; i++) {
                int prime = PRIMES[i];
                int count = 0;
                while (x % prime == 0) {
                    x /= prime;
                    count++;
                }
                if (count > 1) {
                    isValid = false;
                    break;
                }
                if (count == 1) {
                    mask |= (1 << i);
                }
            }
            if (x > 1) isValid = false;
            primeMask[num] = isValid ? mask : -1;
        }
    }


    public static int unitPowerSubsets(int arr[]) {
        int[] freq = new int[31];
        for (int val : arr) {
            freq[val]++;
        }

        long[] dp = new long[1 << PRIMES.length];
        dp[0] = 1;

        for (int num = 2; num <= 30; num++) {
            int mask = primeMask[num];
            if (mask == -1 || freq[num] == 0) continue;

            for (int count = 0; count < freq[num]; count++) {
                long[] newDp = Arrays.copyOf(dp, dp.length);
                for (int oldMask = 0; oldMask < dp.length; oldMask++) {
                    if ((oldMask & mask) == 0) {
                        int combined = oldMask | mask;
                        newDp[combined] = (newDp[combined] + dp[oldMask]) % MOD;
                    }
                }
            dp = newDp;
            }
        }

        long validSubsets = 0;
        for (int i = 1; i < dp.length; i++) {
            validSubsets = (validSubsets + dp[i]) % MOD;
        }

            validSubsets = validSubsets * powMod(2, freq[1], MOD) % MOD;

            return (int) validSubsets;
        }

        private static long powMod(long base, int exp, int mod) {
            long res = 1;
            while (exp > 0) {
                if ((exp & 1) != 0)
                res = res * base % mod;
                base = base * base % mod;
                exp >>= 1;
            }
            return res;
        }
	}

    private static long powMod(long base, int exp, int mod) {
        long res = 1;
        while (exp > 0) {
            if ((exp & 1) != 0)
                res = res * base % mod;
            base = base * base % mod;
            exp >>= 1;
        }
        return res;
    }
}
```
