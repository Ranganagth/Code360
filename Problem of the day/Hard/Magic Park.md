# Intuition

Bob falls straight down from his initial `x` position and slides along slanted props. When he hits a prop, he slides to the **lower end** of it, and then continues falling. Since no two props intersect or touch, we just need to simulate the fall by repeatedly checking which prop Bob hits next until none are left.

---

# Approach

1. At each stage, Bob is at some `x` coordinate and falling vertically down.
2. For all props, check if the vertical line at Bob's current `x` coordinate intersects the prop segment.
3. If it does, determine the **Y coordinate of the intersection** using line equations and find the **lower Y-endpoint** of the prop.
4. From the intersected prop, slide Bob to the lower endpoint and update the `x` to this new point.
5. Repeat the process until no more props intersect.

**Important conditions:**

* Props are slanted (not vertical or horizontal), guaranteed by the problem.
* For a line segment, check if Bob's x lies between x1 and x2 and if the vertical projection hits the segment.

---

# Complexity

* **Time Complexity:** `O(N^2)` per test case (worst case Bob slides on each of N props one by one).
* **Space Complexity:** `O(1)` (no additional space apart from variables).

---

# Code

```java
public class Solution {
    public static int magicPart(int x, int n, int[] x1, int[] x2, int[] y1, int[] y2) {
        double currentHeight = Double.POSITIVE_INFINITY;

        while (true) {
            int chosenIndex = -1;
            double newY = -1;

            for (int i = 0; i < n; i++) {
                int lx = x1[i], rx = x2[i];
                int ly = y1[i], ry = y2[i];

                if (lx == -1) continue; // already used

                if (x >= lx && x <= rx) {
                    double slope = (ry - ly) * 1.0 / (rx - lx);
                    double yAtX = ly + slope * (x - lx);

                    // only choose props strictly below current height
                    if (yAtX < currentHeight) {
                        if (yAtX > newY) {
                            newY = yAtX;
                            chosenIndex = i;
                        }
                    }
                }
            }

            if (chosenIndex == -1) break;

            // Slide to the lower end of the chosen prop
            int i = chosenIndex;
            if (y1[i] < y2[i]) {
                x = x1[i];
                currentHeight = y1[i];
            } else {
                x = x2[i];
                currentHeight = y2[i];
            }

            // Mark the prop as used
            x1[i] = x2[i] = y1[i] = y2[i] = -1;
        }

        return x;
    }
}

```

---

### Example and Explanation Walkthrough

**Input:**

```
x = 4
n = 2
props = [
    (0, 1, 2, 2),
    (2, 4, 4, 5)
]
```

**Step-by-step:**

* Bob starts at `x = 4`
* He intersects with prop (2, 4, 4, 5), slides to (2,4)
* From `x = 2`, he intersects with (0,1,2,2), slides to (0,1)
* From `x = 0`, he no longer intersects any prop
* **Final position: `x = 0`**

---
