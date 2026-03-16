# Intuition

Prime numbers must occupy **prime indices**, and composite numbers (including `1`) must occupy **non-prime indices**.

Let:

* `P` = number of primes ≤ `N`
* prime indices = also `P`
* non-prime indices = `N - P`

Thus:

* prime numbers can be arranged among prime indices in

  ```
  P!
  ```
* remaining numbers (1 and composites) arranged among non-prime indices in

  ```
  (N - P)!
  ```

Total permutations:

```id="8x1v0h"
P! * (N - P)!
```

---

# Approach

1. Count primes ≤ `N` using sieve.
2. Compute factorial of `P`.
3. Compute factorial of `(N - P)`.
4. Multiply them.

Since `N < 100`, `long` is sufficient.

---

# Complexity

* **Time complexity:** $O(N log log N)$

* **Space complexity:** $O(N)$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    static final long MOD = 1000_000_007;

    public static int findPrimePermutations(int n) {

        int primes = countPrimes(n);

        long pFact = factorial(primes);
        long cFact = factorial(n - primes);

        return (int)((pFact * cFact) % MOD);
    }

    static int countPrimes(int n) {

        boolean[] prime = new boolean[n + 1];
        Arrays.fill(prime, true);

        prime[0] = false;
        prime[1] = false;

        for (int i = 2; i * i <= n; i++) {
            if (prime[i]) {
                for (int j = i * i; j <= n; j += i)
                    prime[j] = false;
            }
        }

        int count = 0;

        for (int i = 2; i <= n; i++)
            if (prime[i]) count++;

        return count;
    }

    static long factorial(int x) {

        long res = 1;

        for (int i = 2; i <= x; i++) {
            res = (res * i) % MOD;
        }

        return res;
    }
};

```

---

# Example walkthrough

### Example

```
N = 4
```

Prime numbers:

```id="s94gvt"
2,3
```

Prime indices:

```id="4i8bzy"
2,3
```

Counts:

```id="i6s7ul"
P = 2
Non-primes = 4 - 2 = 2
```

Permutations:

```id="aqp3po"
2! * 2! = 2 * 2 = 4
```

Result:

```id="suyq7m"
4
```
