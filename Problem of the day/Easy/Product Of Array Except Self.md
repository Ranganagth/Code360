# Intuition

To compute a product array where each index `i` contains the product of all elements except `arr[i]`, a brute-force approach using nested loops would be too slow. Instead, we can build prefix and suffix product arrays to compute the result efficiently.

However, to improve space to **O(1)** (ignoring the result array), we must avoid auxiliary arrays and instead reuse the output array intelligently.

Since values can be large, every product must be taken modulo `10^9 + 7`. We also need to handle special cases such as:
* When the array contains a `0`
* When the array size is `0` or `1`

---

# Approach

1. Compute the total product of all elements (taking modulo at every step).
2. If there are more than one zero in the array, all outputs will be zero.
3. If there’s exactly one zero, only that index will have non-zero product (product of all non-zero elements), rest will be zero.
4. If no zero exists:
   * For each index `i`, the output is: `total_product * modInverse(arr[i]) % MOD`
5. To compute modular inverse, use Fermat's Little Theorem:

   ```
   a^(-1) mod p = a^(p-2) mod p (when p is prime)
   ```

---

# Complexity

* **Time complexity:** `O(N)` per test case
* **Space complexity:** `O(1)` extra space (excluding the result array)

---

# Code

```java
public class Solution {

    private static final int MOD = 1_000_000_007;

    // Fast exponentiation: (a^b) % MOD
    private static long modPow(long a, long b) {
        long result = 1;
        a = a % MOD;
        while (b > 0) {
            if ((b & 1) == 1) result = (result * a) % MOD;
            a = (a * a) % MOD;
            b >>= 1;
        }
        return result;
    }

    // Modular inverse using Fermat's Little Theorem
    private static long modInverse(long a) {
        return modPow(a, MOD - 2);
    }

    public static int[] getProductArrayExceptSelf(int[] arr) {
        int n = arr.length;
        int[] product = new int[n];

        long totalProduct = 1;
        int zeroCount = 0;

        // First pass: compute total product and count zeros
        for (int value : arr) {
            if (value == 0) {
                zeroCount++;
            } else {
                totalProduct = (totalProduct * value) % MOD;
            }
        }

        // Second pass: handle cases based on number of zeros
        for (int i = 0; i < n; i++) {
            if (zeroCount > 1) {
                product[i] = 0;
            } else if (zeroCount == 1) {
                product[i] = (arr[i] == 0) ? (int) totalProduct : 0;
            } else {
                product[i] = (int) ((totalProduct * modInverse(arr[i])) % MOD);
            }
        }

        return product;
    }
}
```

---

### Sample Execution

**Input:**

```java
int[] arr = {1, 2, 3};
```

**Output:**

```
[6, 3, 2]
```

**Input:**

```java
int[] arr = {5, 2, 2};
```

 **Output:**

```
[4, 10, 10]
```

---
