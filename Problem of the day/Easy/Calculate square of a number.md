# Intuition

We need to compute the square of a number without directly using multiplication (`*`), division (`/`), or power functions (`pow()`).
The square of a number `n` means `n × n`. Since multiplication is restricted, we can simulate it using repeated addition.
However, directly adding `n` `n` times would be too slow (`O(n)`). Instead, we can use **bit manipulation and the Russian Peasant Multiplication technique** (repeated doubling and halving) to perform multiplication efficiently in `O(log n)` time.

So the main idea:

* Implement a custom multiplication function using bitwise operations and addition.
* Use it to calculate `n * n`.

---

# Approach

1. Handle negative input: take the absolute value since squaring always results in a non-negative number.
2. Implement a helper method `multiply(a, b)` that computes multiplication using addition and bit shifts:

   * While `b > 0`:

     * If the last bit of `b` is set (`b & 1`), add `a` to the result.
     * Double `a` (`a << 1`) and halve `b` (`b >> 1`).
3. Call `multiply(num, num)` to compute the square.
4. Return the result.

---

# Complexity

* **Time Complexity:** `O(log n)` (where `n` is the absolute value of the number) because we reduce `b` by half at each step in multiplication.
* **Space Complexity:** `O(1)` (constant extra space).

---

# Code

```java
import java.util.*;

public class Solution 
{
    public static int calculateSquare(int num) 
    {
        // Square is always positive, so take absolute value
        num = Math.abs(num);
        return multiply(num, num);
    }

    // Custom multiplication using addition and bit manipulation
    private static int multiply(int a, int b) {
        int result = 0;
        while (b > 0) {
            // If b is odd, add current 'a' to result
            if ((b & 1) == 1) {
                result += a;
            }
            // Double 'a' and halve 'b'
            a <<= 1;
            b >>= 1;
        }
        return result;
    }

    // For testing multiple cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        while (T-- > 0) {
            int n = sc.nextInt();
            System.out.println(calculateSquare(n));
        }
        sc.close();
    }
}
```

---

# Example Walkthrough

**Input:**

```
2
3
-4
```

**Step 1:** First test case, `num = 3`.

* We call `multiply(3, 3)`.

  * Initially result = 0, a = 3, b = 3 (binary `11`).
  * `b & 1 = 1`, so result += 3 → result = 3.
  * a = 6, b = 1.
  * `b & 1 = 1`, so result += 6 → result = 9.
  * a = 12, b = 0 → stop.
* Final result = 9.

**Step 2:** Second test case, `num = -4`.

* Take absolute value → `num = 4`.
* Call `multiply(4, 4)`.

  * Initially result = 0, a = 4, b = 4 (binary `100`).
  * `b & 1 = 0`, skip. a = 8, b = 2.
  * `b & 1 = 0`, skip. a = 16, b = 1.
  * `b & 1 = 1`, result += 16 → result = 16.
  * a = 32, b = 0 → stop.
* Final result = 16.

**Output:**

```
9
16
```

---

