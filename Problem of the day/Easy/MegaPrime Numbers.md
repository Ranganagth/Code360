# Intuition

We need to count **megaprime** numbers in a given range. A number is megaprime if:

1. It is a **prime number**, and
2. **Each of its digits is also a prime digit** (i.e., in the set {2, 3, 5, 7})

So we must:

* Check all numbers in the range.
* First, check if a number is prime.
* Then, check if **all its digits** are also prime digits.

# Approach

1. **Generate all prime numbers up to 8000** using the **Sieve of Eratosthenes**, since the maximum `right` is 8000.
2. **Store all prime numbers in a `Set`** for quick lookup.
3. For each number in the range `[left, right]`:

   * If it's in the prime set,
   * Convert it to a string or extract digits and verify that each digit is in `{2, 3, 5, 7}`.
   * If so, count it as a megaprime.
4. Repeat the above for each test case.

# Complexity

* **Time complexity**:
  * Preprocessing with Sieve of Eratosthenes: $O(n \log \log n)$ where $n = 8000$
  * For each test case:
    * Checking all numbers in range: $O(R - L)$
    * Digit check per number: $O(\log_{10}(n))$ per number
  * Total (worst case for all test cases): $O(T \cdot (R - L) \cdot \log_{10}(n))$, which is efficient under constraints

* **Space complexity**:
  * $O(n)$ for storing sieve and set of primes

# Code

```java
import java.util.*;

public class Solution {
    private static final int MAX = 8000;
    private static final Set<Integer> primeDigits = Set.of(2, 3, 5, 7);
    private static final boolean[] isPrime = new boolean[MAX + 1];

    // Static block to initialize prime numbers using sieve
    static {
        Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;
        for (int i = 2; i * i <= MAX; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j <= MAX; j += i) {
                    isPrime[j] = false;
                }
            }
        }
    }

    public static int countMegaPrimeNumber(int left, int right) {
        int count = 0;
        for (int num = left; num <= right; num++) {
            if (isPrime[num] && hasAllPrimeDigits(num)) {
                count++;
            }
        }
        return count;
    }

    private static boolean hasAllPrimeDigits(int num) {
        while (num > 0) {
            int digit = num % 10;
            if (!primeDigits.contains(digit)) {
                return false;
            }
            num /= 10;
        }
        return true;
    }
}
```
