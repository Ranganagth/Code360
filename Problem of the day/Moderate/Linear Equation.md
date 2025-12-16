# Intuition

The equation
$$a x + b y = c$$
has an **integer solution if and only if** `gcd(a, b)` divides `c`.

If we can find **one solution** to
$$a x + b y = \gcd(a,b)$$
then multiplying both `x` and `y` by `c / gcd(a,b)` gives a solution to the required equation.

So the core task reduces to computing **Extended Euclidean Algorithm**, which directly gives integers `x` and `y` satisfying:
$$a x + b y = \gcd(a,b)$$

---

# Approach

1. Compute `g = gcd(a, b)` using **Extended Euclid**.
2. If `c % g != 0`, no integer solution exists → return `[-1, -1]`.
3. Otherwise:

   * Scale the coefficients obtained from Extended Euclid by `c / g`.
   * The resulting `(x, y)` is a valid **initial integral solution**.
4. Return the computed `(x, y)`.

This gives a valid solution directly, without searching.

---

# Complexity

* **Time Complexity:** `O(log(min(a, b)))`
* **Space Complexity:** `O(log(min(a, b)))` (recursion stack)

---

# Code

```java
public class Solution {

    static class Pair {
        long x, y, gcd;
        Pair(long x, long y, long gcd) {
            this.x = x;
            this.y = y;
            this.gcd = gcd;
        }
    }

    private static Pair extendedGCD(long a, long b) {
        if (b == 0) {
            return new Pair(1, 0, a);
        }

        Pair next = extendedGCD(b, a % b);
        long x = next.y;
        long y = next.x - (a / b) * next.y;

        return new Pair(x, y, next.gcd);
    }

    public static int[] linearEquation(int a, int b, int c) {
        Pair res = extendedGCD(a, b);

        if (c % res.gcd != 0) {
            return new int[]{-1, -1};
        }

        long factor = c / res.gcd;
        long x = res.x * factor;
        long y = res.y * factor;

        return new int[]{(int) x, (int) y};
    }
};

```

---

# Example Walkthrough

### Example 1

**Input:**
`a = 5, b = 10, c = 10`

* `gcd(5,10) = 5`
* `10 % 5 == 0` → solution exists
* Extended GCD gives:

  ```
  5(1) + 10(0) = 5
  ```
* Scale by `10 / 5 = 2`:

  ```
  x = 1 * 2 = 2
  y = 0 * 2 = 0
  ```

**Output:**

```
2 0
```


### Example 2

**Input:**
`a = 4, b = 6, c = 1`

* `gcd(4,6) = 2`
* `1 % 2 != 0` → no solution

**Output:**

```
-1 -1
```

---
