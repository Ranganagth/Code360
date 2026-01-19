# Intuition

The monotonic stack approach is correct in direction but needs refinement. When we pop **equal-height** hills, we must account for **multiple soldiers of the same height** forming visible pairs with the current hill and with each other.

The earlier implementation only counted one visibility per pop, missing combinations formed by consecutive equal-height hills.

---

# Approach

Maintain a stack of **pairs (height, count)** where `count` represents how many consecutive hills of that height exist contiguously and visible up to this point.

**Algorithm:**

1. For each hill `h`:

   * Initialize `visible = 1` (current hill itself).
   * While stack top has height ≤ `h`:

     * Pop the top `(height, freq)`
     * Add `freq` to `count` — all hills of that height are visible with the current hill.
     * If top height == current height, accumulate same height count (`visible += freq`).
   * If stack not empty after popping, add `1` to `count` because current hill can see the next taller one in stack.
   * Push `(h, visible)` onto stack.
2. Return total `count`.

This correctly handles consecutive equal heights and tall hill boundaries.

---

# Complexity

* **Time complexity:** O(N)
  Each hill is pushed and popped once.
* **Space complexity:** O(N)
  Stack stores pairs of heights and frequencies.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int countPairs(int n, int[] hills) {
        Stack<int[]> stack = new Stack<>();
        int count = 0;

        for (int i = 0; i < n; i++) {
            int h = hills[i];
            int same = 1;

            while (!stack.isEmpty() && stack.peek()[0] <= h) {
                int[] top = stack.pop();
                count += top[1]; // all visible with current

                if (top[0] == h) {
                    same += top[1]; // merge same height group
                }
            }

            if (!stack.isEmpty()) {
                count++; // current hill sees next taller
            }

            stack.push(new int[]{h, same});
        }

        return count;
    }
};

```

---

# Example walkthrough with explanation

**Input:**
`N = 5`
`HILLS = [3, 2, 1, 3, 5]`

| Step | Hill | Stack before        | Visible pairs added                                                         | Stack after         |
| ---- | ---- | ------------------- | --------------------------------------------------------------------------- | ------------------- |
| 1    | 3    | []                  | -                                                                           | [(3,1)]             |
| 2    | 2    | [(3,1)]             | +1 (3↔2)                                                                    | [(3,1),(2,1)]       |
| 3    | 1    | [(3,1),(2,1)]       | +1 (2↔1)                                                                    | [(3,1),(2,1),(1,1)] |
| 4    | 3    | [(3,1),(2,1),(1,1)] | pop 1 (+1), pop 2 (+1), pop nothing for equal height, +1 for 3↔3 → total +3 | [(3,2)]             |
| 5    | 5    | [(3,2)]             | pop 3 (+2), +0 taller left → total +2                                       | [(5,1)]             |

Total pairs = 1 + 1 + 3 + 2 = **7**
Matches expected output.
