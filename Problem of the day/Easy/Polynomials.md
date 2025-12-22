# Intuition

A polynomial is just a sequence of coefficients where the index represents the power of `x`.

Example:
`A = [1, 2, 3]` represents
`1·x² + 2·x¹ + 3·x⁰`

When two polynomials are multiplied, **every term of the first polynomial multiplies with every term of the second**.
The powers add up, and coefficients contributing to the same power are summed.

This is exactly the same as **array convolution**.

---

# Approach

1. Let both polynomials have length `n` (degree `n-1`)
2. The resulting polynomial will have:

   ```
   size = (n - 1) + (n - 1) + 1 = 2n - 1
   ```
3. Create a result array of size `2n - 1` initialized with `0`
4. Use two nested loops:

   * Loop over all coefficients of `A`
   * Loop over all coefficients of `B`
   * Add `A[i] * B[j]` to `result[i + j]`
5. Return the result array

This directly follows the mathematical definition of polynomial multiplication.

---

# Algorithm Steps

1. Initialize `result[2n - 1]`
2. For `i = 0 → n-1`
   * For `j = 0 → n-1`
     * `result[i + j] += A[i] * B[j]`
3. Return `result`

---

# Complexity

* **Time:** `O(n²)`
  Every coefficient of `A` multiplies with every coefficient of `B`
* **Space:** `O(n)`
  Only the result array of size `2n - 1` is used

This fits the constraints since `n ≤ 10⁴` and `T ≤ 10`.

---

# Code

```java
public class Solution {
    public static int[] multiply(int[] a, int[] b, int n) {
        int size = 2 * n - 1;
        long[] temp = new long[size];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                temp[i + j] += (long) a[i] * b[j];
            }
        }

        int[] result = new int[size];
        for (int i = 0; i < size; i++) {
            result[i] = (int) temp[i];
        }

        return result;
    }
};

```

---

# Example Walkthrough

**Input**

```
A = [1, 2, 3]
B = [3, 2, 1]
```

Represents:

```
A = x² + 2x + 3
B = 3x² + 2x + 1
```

**Result array size**

```
2n - 1 = 5
result = [0, 0, 0, 0, 0]
```

**Multiplication steps**

| i | j | a[i] | b[j] | a[i]*b[j] | index | result       |
| - | - | ---- | ---- | --------- | ----- | ------------ |
| 0 | 0 | 1    | 3    | 3         | 0     | [3,0,0,0,0]  |
| 0 | 1 | 1    | 2    | 2         | 1     | [3,2,0,0,0]  |
| 0 | 2 | 1    | 1    | 1         | 2     | [3,2,1,0,0]  |
| 1 | 0 | 2    | 3    | 6         | 1     | [3,8,1,0,0]  |
| 1 | 1 | 2    | 2    | 4         | 2     | [3,8,5,0,0]  |
| 1 | 2 | 2    | 1    | 2         | 3     | [3,8,5,2,0]  |
| 2 | 0 | 3    | 3    | 9         | 2     | [3,8,14,2,0] |
| 2 | 1 | 3    | 2    | 6         | 3     | [3,8,14,8,0] |
| 2 | 2 | 3    | 1    | 3         | 4     | [3,8,14,8,3] |

**Final Output**

```
[3, 8, 14, 8, 3]
```

Which corresponds to:

```
3x⁴ + 8x³ + 14x² + 8x + 3
```



---
