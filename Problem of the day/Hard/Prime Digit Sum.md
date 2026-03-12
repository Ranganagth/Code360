# Intuition

A number is **good** if the **sum of its digits is prime**.
Given that $(R \le 10^5)$, the maximum digit sum is:

```id="sxzq2o"
99999 → 9+9+9+9+9 = 45
```

So only primes up to **45** need to be checked.

Thus:

1. Precompute primes up to `45`.
2. Iterate from `L` to `R`.
3. Compute digit sum and check if it is prime.

---

# Approach

1. Generate a boolean array `isPrime[46]` using sieve.
2. For each number `i` from `L` to `R`:

   * compute digit sum.
   * if `isPrime[sum]` → increment count.
3. Return the count.

---

# Complexity

* **Time complexity:** $(O((R-L+1) \times \text{digits})) ≈ (O(R-L))$
* **Space complexity:** $(O(1))$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    static int primeDigitSum(int l, int r){
        boolean[] prime = sieve(46);

        int count = 0;

        for (int i = l; i <= r; i++) {
            int sum = digitSum(i);
            if (prime[sum]) count++;
        }

        return count;        
    }

    static int digitSum(int n) {
        int sum = 0;

        while(n > 0) {
            sum += n % 10;
            n /= 10;
        }

        return sum;
    }

    static boolean[] sieve(int n) {

        boolean[] prime = new boolean[n];
        Arrays.fill(prime, true);

        prime[0] = false;
        prime[1] = false;

        for (int i = 2; i * i < n; i++) {

            if (prime[i]) {

                for (int j = i * i; j < n; j += i) {
                    prime[j] = false;
                }
            }
        }

        return prime;
    }
};

```

---

# Example walkthrough with explanation

Example:

```id="4w82yp"
L = 15
R = 19
```

Numbers:

```id="ahmxt0"
15 → 1+5 = 6  (not prime)
16 → 1+6 = 7  (prime)
17 → 1+7 = 8  (not prime)
18 → 1+8 = 9  (not prime)
19 → 1+9 = 10 (not prime)
```

Good numbers:

```id="38jymq"
16
```

Output:

```id="ej28rq"
1
```
