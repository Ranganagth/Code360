# Intuition
We are given integers and we need to:
- Identify which numbers have **exactly 4 divisors**.
- For those numbers, compute the **sum of all their divisors**.
- Return the total sum for each test case.

## Observation
A number will have **exactly 4 divisors** if and only if:
1. It is the product of two **distinct prime numbers** → e.g. 6 = 2×3 → divisors: 1, 2, 3, 6
2. It is the cube of a **prime number** → e.g. 8 = 2³ → divisors: 1, 2, 4, 8

We leverage this to avoid checking every divisor explicitly.

---

# Approach
1. Precompute all **prime numbers** up to √(10⁵) using the Sieve of Eratosthenes.
2. For each number in `arr`:
   - Check if it is the product of two distinct primes: `p × q` where `p ≠ q`.
   - Or check if it's a cube of a prime: `p³`.
3. If yes, calculate its 4 divisors and their sum.

---

# Complexity

- **Time complexity:**  
  - Precomputation: $O(n log log n)$
  - Per test case: $O(N × √max(arr[i]))$ ≈ acceptable for constraints
  
- **Space complexity:** $O(n)$ for prime list

---

# Code

```java
import java.util.*;

public class Solution {
    
    static boolean[] isPrime = new boolean[100001];

    // Sieve of Eratosthenes to precompute primes
    static void sieve() {
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    int n = isPrime.length;
    for (int i = 2; i * i < n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j < n; j += i) {
                isPrime[j] = false;
            }
        }
    }
}


    public static int sumFourDivisors(ArrayList<Integer> arr, int n) {
        sieve(); // run once
        int totalSum = 0;

        for (int num : arr) {
            int sum = getSumIfExactly4Divisors(num);
            totalSum += sum;
        }

        return totalSum;
    }

    private static int getSumIfExactly4Divisors(int num) {
        // Case 1: num = p^3
        int root = (int)Math.round(Math.pow(num, 1.0 / 3));
        if (root * root * root == num && isPrime[root]) {
            return 1 + root + root * root + num;
        }

        // Case 2: num = p * q, p != q
        for (int i = 2; i * i <= num; i++) {
            if (num % i == 0) {
                int j = num / i;
                if (i != j && isPrime[i] && isPrime[j]) {
                    return 1 + i + j + num;
                }
            }
        }

        return 0; // not a 4-divisor number
    }
}
```

---

### Example

For input:
```
2
4
2 5 6 15
3
4 18 21
```

Output:
```
36
32
```
