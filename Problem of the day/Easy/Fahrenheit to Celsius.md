# Intuition

To convert Fahrenheit to Celsius, we use the formula:

$$
C = \left\lfloor \frac{5}{9} \times (F - 32) \right\rfloor
$$

Where:
* $C$ is the Celsius temperature.
* $F$ is the Fahrenheit temperature.
* The result is **floored** to the nearest integer.

We loop from **`S` to `E`** with step size **`W`**, apply the formula, and store the pairs `[F, C]`.

---

# Approach

1. Initialize a list to store all pairs `[F, C]`.
2. Use a loop from `S` to `E`, incrementing by `W`.
3. At each step, compute `C = (int) Math.floor((5.0 / 9) * (F - 32))`.
4. Add `[F, C]` to the result list.

---

# Complexity Analysis

* **Time Complexity:** O(N), where N = number of steps from S to E with step size W.
* **Space Complexity:** O(N), for storing the output list.

---

# Code

```java
import java.util.*;

public class Solution {

    public static List<List<Integer>> fahrenheitToCelsius(int s, int e, int w) {
        List<List<Integer>> result = new ArrayList<>();

        for (int f = s; f <= e; f += w) {
            int c = (int)((5.0 / 9) * (f - 32));
            List<Integer> pair = new ArrayList<>();
            pair.add(f);
            pair.add(c);
            result.add(pair);
        }

        return result;
    }
}
```

---

## Example Execution

For input:

```java
fahrenheitToCelsius(0, 100, 20)
```

Output will be:

```java
[
 [0, -17],
 [20, -6],
 [40, 4],
 [60, 15],
 [80, 26],
 [100, 37]
]
```

Each sublist is one line in the required output format.
