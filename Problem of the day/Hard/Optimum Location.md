# Intuition

You need a point on line:

$$ax + by + c = 0
$$

such that **sum of Euclidean distances to given points is minimized**.

This is a **convex optimization problem on a line** → single variable optimization.

Convert the 2D line into a **1D parameter**:

* express `(x, y)` on line using one variable
* minimize function:

$$f(x) = Σ distance from (x, y(x)) to given points
$$
Use **ternary search** because function is convex.

---

# Approach

1. Express `y` in terms of `x`:
$$y = (-a*x - c) / b
$$
2. Define function:
$$f(x) = Σ sqrt((x - xi)^2 + (y - yi)^2)
$$

3. Apply **ternary search** on x in range `[-1e4, 1e4]`
4. Iterate ~100 times for precision
5. Return result

---

# Complexity

* **Time complexity:**
$$O(100 * N)
$$
* **Space complexity:**
$$O(1)
$$
---

# Code

```java
import java.util.*;

public class Solution {

    public static double optimumDistance(int a, int b, int c,
                                         ArrayList<ArrayList<Integer>> points, int n) {

        double left = -1e4, right = 1e4;

        for (int i = 0; i < 100; i++) {

            double m1 = left + (right - left) / 3.0;
            double m2 = right - (right - left) / 3.0;

            double d1 = compute(m1, a, b, c, points);
            double d2 = compute(m2, a, b, c, points);

            if (d1 < d2) {
                right = m2;
            } else {
                left = m1;
            }
        }

        return compute((left + right) / 2, a, b, c, points);
    }

    private static double compute(double x, int a, int b, int c,
                                  ArrayList<ArrayList<Integer>> points) {

        double y = (-a * x - c) / (double) b;

        double sum = 0;

        for (ArrayList<Integer> p : points) {
            double dx = x - p.get(0);
            double dy = y - p.get(1);
            sum += Math.sqrt(dx * dx + dy * dy);
        }

        return sum;
    }
};

```

---

# Example

Line: x - y = 0 → y = x

Points: (-1,1), (1,-1)

Optimal x = 0 → (0,0)

---
# Key Insight

- 2D optimization → reduce to 1D using line equation
- Convex function → ternary search guarantees minimum

---
