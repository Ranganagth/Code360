# Intuition

We are asked to find **pairs (a, b) that are coprime-twins**, meaning:

> For all positive integers `x`, `gcd(a, x) = 1` iff `gcd(b, x) = 1`.

This is equivalent to saying:

* `S(a) = S(b)`, where `S(x)` is the set of integers coprime with `x`.

Now, two numbers have the same coprime set **if and only if they have the same set of prime factors** (ignoring exponents).

* Example: `2` and `4` → both divisible by prime `2` → same prime factor set `{2}` → coprime-twins.
* Example: `6 (2*3)` and `12 (2*2*3)` → both divisible by primes `{2,3}` → same prime factor set `{2,3}` → coprime-twins.
* Example: `1` and `2` → prime factors of `1` is `{}` (empty set), prime factors of `2` is `{2}` → not coprime-twins.

So the **coprime-twin relation is determined solely by the square-free kernel** of each number:
`kernel(x) = product of distinct primes dividing x`.

---

# Approach

1. **Precompute smallest prime factors (SPF) for factorization** up to `10^6` using a sieve.
2. For each number in the array, compute its **square-free kernel**.

   * Example: `12 → 2^2 * 3 → distinct primes = {2,3} → kernel = 6`.
   * Example: `18 → 2 * 3^2 → kernel = 6`.
   * Example: `4 → 2^2 → kernel = 2`.
3. Replace each number in array with its kernel.
   Then, two numbers are coprime-twins **iff they have the same kernel**.
4. For each query `[L, R]`, we need to count pairs `(i, j)` where `kernel[a[i]] = kernel[a[j]]`.

   * That’s just: for each distinct kernel in the range, if it appears `f` times, it contributes `f * (f-1) / 2` pairs.
5. To answer up to `10^4` queries efficiently:

   * **Naïve approach:** O(N \* Q) (too slow).
   * **Better approach:** Use **Mo’s algorithm** (offline query processing, sqrt-decomposition).

     * Maintain frequency of each kernel in the current range.
     * Maintain current count of valid pairs.
     * Add/remove elements as we move the range.

---

# Complexity

* Preprocessing SPF (sieve): **O(N log log N)** up to `10^6`.
* Kernel computation per number: **O(log Ai)**.
* Mo’s algorithm: **O((N + Q) \* sqrt(N))** operations.
* This fits within constraints (`N, Q ≤ 10^4`).

---

# Java

```java
import java.util.*;

public class Solution {

    static int MAX = 1000000;
    static int[] spf = new int[MAX + 1];

    // Precompute smallest prime factor
    static void sieve() {
        for (int i = 2; i <= MAX; i++) {
            if (spf[i] == 0) {
                for (int j = i; j <= MAX; j += i) {
                    if (spf[j] == 0) spf[j] = i;
                }
            }
        }
    }

    // Compute square-free kernel
    static int kernel(int n) {
        int result = 1;
        while (n > 1) {
            int p = spf[n];
            result *= p;
            while (n % p == 0) n /= p;
        }
        return result;
    }

    static class Query {
        int l, r, idx;
        Query(int l, int r, int idx) {
            this.l = l; this.r = r; this.idx = idx;
        }
    }

    public static int[] coprimeTwins(int[] a, int[][] queries) {
        int n = a.length;
        int q = queries.length;

        sieve();

        // Replace array with kernels
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            arr[i] = kernel(a[i]);
        }

        // Prepare queries for Mo's algorithm
        int block = (int) Math.sqrt(n);
        Query[] qs = new Query[q];
        for (int i = 0; i < q; i++) {
            qs[i] = new Query(queries[i][0]-1, queries[i][1]-1, i);
        }

        Arrays.sort(qs, (x, y) -> {
            int bx = x.l / block, by = y.l / block;
            if (bx != by) return bx - by;
            return (bx % 2 == 0) ? x.r - y.r : y.r - x.r;
        });

        long[] ans = new long[q];
        Map<Integer, Integer> freq = new HashMap<>();
        long currPairs = 0;

        int L = 0, R = -1;
        for (Query qu : qs) {
            int l = qu.l, r = qu.r;

            while (R < r) {
                R++;
                int val = arr[R];
                int f = freq.getOrDefault(val, 0);
                currPairs += f; // add new pairs
                freq.put(val, f+1);
            }
            while (R > r) {
                int val = arr[R];
                int f = freq.getOrDefault(val, 0);
                currPairs -= (f-1);
                if (f == 1) freq.remove(val);
                else freq.put(val, f-1);
                R--;
            }
            while (L < l) {
                int val = arr[L];
                int f = freq.getOrDefault(val, 0);
                currPairs -= (f-1);
                if (f == 1) freq.remove(val);
                else freq.put(val, f-1);
                L++;
            }
            while (L > l) {
                L--;
                int val = arr[L];
                int f = freq.getOrDefault(val, 0);
                currPairs += f;
                freq.put(val, f+1);
            }

            ans[qu.idx] = currPairs;
        }

        return Arrays.stream(ans).mapToInt(x -> (int)x).toArray();
    }
}
```

---
# Example Walkthrough

Input:

```
A = [2, 4, 6, 8, 6, 4]
Queries:
(3,5), (2,4), (1,4), (1,5)
```

Step 1: Compute kernels:

* 2 → {2} → 2
* 4 → {2} → 2
* 6 → {2,3} → 6
* 8 → {2} → 2
* 6 → {2,3} → 6
* 4 → {2} → 2

Kernel array: `[2, 2, 6, 2, 6, 2]`

Step 2: Answer queries:

* Query (3,5) → `[6,2,6]` → freq: {6:2, 2:1} → pairs = 2C2 = 1
* Query (2,4) → `[2,6,2]` → freq: {2:2, 6:1} → pairs = 1
* Query (1,4) → `[2,2,6,2]` → freq: {2:3, 6:1} → pairs = 3C2 = 3
* Query (1,5) → `[2,2,6,2,6]` → freq: {2:3, 6:2} → pairs = 3C2 + 2C2 = 3+1=4

Matches sample output.

---
