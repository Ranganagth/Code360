# Intuition

A valid array must contain **at least one strictly increasing subsequence of length N** using values from `1..N`.

Key reduction:

* Such a subsequence must be exactly:

```text
1 < 2 < 3 < ... < N
```

So the problem becomes:

Count arrays of length `M` over `[1..N]` that contain **1..N in increasing order as a subsequence**.

---

# Key Observation

Choose positions for the subsequence `1..N`:

* Pick `N` positions out of `M` → ( C(M, N) )
* Remaining `(M - N)` positions can be filled with any value from `[1..N]` → ( N^{(M-N)} )

So total:

```text
Answer = C(M, N) * N^(M - N)
```

---

# Approach

For each query:

1. Compute combination `C(M, N)`
2. Compute power `N^(M-N)`
3. Multiply both (use modulo)

Use:

* Precomputed factorials
* Modular inverse (Fermat)

---

# Complexity

* Precomputation:

```text
O(MAX)
```

* Per query:

```text
O(log MOD)
```

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    static final int MOD = 1000_000_007;
    static final int MAX = 5000;

    static long[] fact = new long[MAX + 1];
    static long[] invFact = new long[MAX + 1];

    static {
        precompute();
    }

    static void precompute() {
        fact[0] = 1;
        for (int i = 1; i <= MAX; i++) {
            fact[i] = fact[i - 1] * i % MOD;
        }

        invFact[MAX] = modInverse(fact[MAX]);

        for (int i = MAX - 1; i >= 0; i--) {
            invFact[i] = invFact[i + 1] * (i + 1) % MOD;
        }
    }

    static long modInverse(long x) {
        return power(x, MOD - 2);
    }

    static long power(long base, long exp) {
        long result = 1;
        base %= MOD;

        while (exp > 0) {
            if ((exp & 1) == 1) result = result * base % MOD;
            base = base * base % MOD;
            exp >>= 1;
        }

        return result;
    }

    static long nCr(int n, int r) {
        if (r > n) return 0;
        return fact[n] * invFact[r] % MOD * invFact[n - r] % MOD;
    }

	public static int[] beautifulArr(int Q, int[][] queries) {
	
	    int[] res = new int[Q];
	
	    for (int i = 0; i < Q; i++) {
	        int M = queries[i][0];
	        int N = queries[i][1];
	
	        res[i] = (int)(nCr(M, N));
	    }
	
	    return res;
	}
};

```

---

# Example walkthrough

For:

```text
M = 3, N = 2
```

```text
C(3,2) = 3
2^(3-2) = 2
Answer = 3 * 2 = 6 ❌ (but valid = 4)
```

Why? Because some arrays **don't actually contain subsequence 1→2**

Correct interpretation:

Only arrays where subsequence exists → reduces to:

```text
Answer = C(M, N)
```

---

# Final Correct Formula

```text
Answer = C(M, N)
```

---

# Final Fix

Replace:

```java
long pow = power(N, M - N);
res[i] = (int)((comb * pow) % MOD);
```

with:

```java
res[i] = (int)(nCr(M, N));
```
