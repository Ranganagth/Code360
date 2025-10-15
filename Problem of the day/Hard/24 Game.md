# Intuition

We are given **4 numbers (each from 1–9)**.
We must determine whether it’s possible to make **24** by using:

* the numbers **exactly once**,
* **operators**: `+`, `-`, `*`, `/`,
* and **any parentheses** (i.e., any order of operations).

This is a **search problem** — we must try all possible combinations of:

1. Number pairings,
2. Operator choices,
3. Parentheses placements (implicitly handled by recursion order).

Because there are only **4 numbers**, brute force with pruning is acceptable.

---

# Approach

We can use **backtracking with floating-point arithmetic**:

1. **Base case**:

   * If only one number is left:

     * Check if it is approximately equal to 24 (since floating-point division may not be exact).
     * Use a small epsilon (e.g., `1e-6`) for tolerance.

2. **Recursive case**:

   * For each **pair of numbers (a, b)**:

     * Remove them from the list.
     * Apply all possible operations between them:

       * `a + b`
       * `a - b`
       * `b - a`
       * `a * b`
       * `a / b` (if `b != 0`)
       * `b / a` (if `a != 0`)
     * Add the result back to the list and recurse.

3. If any combination yields 24 → return `true`.

4. If all fail → return `false`.

---

# Complexity Analysis

* Each recursive step reduces the number count by 1.
* For 4 numbers → total combinations are small.
* Roughly **O(4! × 4³)**, well within limits.

---

# Code

```java
public class Solution {

    public static Boolean judge(int[] nums) {
        double[] arr = new double[nums.length];
        for (int i = 0; i < nums.length; i++) {
            arr[i] = nums[i];
        }
        return helper(arr);
    }

    private static boolean helper(double[] nums) {
        int n = nums.length;
        if (n == 1) {
            return Math.abs(nums[0] - 24.0) < 1e-6;
        }

        // Try all pairs
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) continue;

                // Prepare the next array after using nums[i] and nums[j]
                double[] next = new double[n - 1];
                int index = 0;
                for (int k = 0; k < n; k++) {
                    if (k != i && k != j) {
                        next[index++] = nums[k];
                    }
                }

                // Try all possible results of combining nums[i] and nums[j]
                for (double val : compute(nums[i], nums[j])) {
                    next[n - 2] = val; // place new result
                    if (helper(next)) return true;
                }
            }
        }
        return false;
    }

    private static double[] compute(double a, double b) {
        return new double[]{
            a + b,
            a - b,
            b - a,
            a * b,
            (Math.abs(b) > 1e-6) ? a / b : Double.NaN,
            (Math.abs(a) > 1e-6) ? b / a : Double.NaN
        };
    }
};

```

---

## Example Walkthrough

### Input

```
nums = [4, 1, 8, 7]
```

### Process

* Choose (8, 4) → try (8 - 4) = 4.
* Remaining numbers: [4, 1, 7]
* Choose (7, 1) → (7 - 1) = 6.
* Remaining: [4, 6]
* Multiply → 4 * 6 = 24.

Return `true`.

---

## Examples

### Input

```
2
4 1 8 7
1 2 1 2
```

### Output

```
True
False
```

---

