# Intuition

We need the **smallest step size `jump`** such that when we repeatedly jump from `0` in increments of `jump`, we **never land on any obstacle**.

Key observations:

1. If an obstacle coordinate is divisible by the `jump`, then we will land on it.
    So, `jump` must not divide any obstacle position.
2. The end point is just **greater than the maximum obstacle**, so we only need to worry about avoiding obstacles up to `max(obstacles)`.

So the task reduces to:

* Find the smallest integer `jump ≥ 2` such that for all `obs` in `obstacles`, `obs % jump != 0`.

---

# Approach

1. Compute `maxVal = max(obstacles)`.
   The end point is `maxVal + 1`.
2. Try `jump = 2, 3, 4, ...`
   For each `jump`, check:

   * For every obstacle, ensure `obs % jump != 0`.
   * If true, return this `jump`.
3. This works because at most, the required jump will be **greater than maxVal** (then it skips all obstacles in one go).

---

# Complexity

* Worst case: We may check up to `O(maxVal)` jumps.
* For each jump, we check `N` obstacles.
* Time complexity = **O(N \* jump)** in worst case.
  But since constraints are `N ≤ 1000`, and obstacle values up to `1e6`, this is acceptable within `1 sec` because we usually find small jumps quickly.

---

# Code

```java
import java.util.* ;
import java.io.*; 

class Solution {
    public static int avoidTraps(ArrayList<Integer> obstacles, int n) {
        int maxVal = 0;
        for (int obs : obstacles) {
            if (obs > maxVal) maxVal = obs;
        }

        // try jumps starting from 2
        for (int jump = 2; ; jump++) {
            boolean valid = true;
            for (int obs : obstacles) {
                if (obs % jump == 0) {
                    valid = false;
                    break;
                }
            }
            if (valid) return jump;
        }
    }
}
```

---

# Example Walkthrough

### Example 1

```
obstacles = [5, 3, 6, 7, 9]
maxVal = 9
Try jump=2 → 6 % 2 == 0 → invalid
Try jump=3 → 3 % 3 == 0 → invalid
Try jump=4 → none of 5,3,6,7,9 divisible by 4 → valid
Answer = 4
```

### Example 2

```
obstacles = [5, 8, 9, 13, 14]
maxVal = 14
Try jump=2 → 8 % 2 == 0 → invalid
Try jump=3 → 9 % 3 == 0 → invalid
Try jump=4 → 8 % 4 == 0 → invalid
Try jump=5 → 5 % 5 == 0 → invalid
Try jump=6 → none divisible → valid
Answer = 6
```

---

