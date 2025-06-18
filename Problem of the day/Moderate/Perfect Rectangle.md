# Intuition

If multiple rectangles form a perfect rectangle:

* The total area of small rectangles should be equal to the area of the large rectangle they are forming.
* The corners of all small rectangles should pair up **except for the four outer corners** of the final big rectangle.
* If any point occurs odd number of times **except these 4 corners**, then it implies either overlap or gap.

---

# Approach

1. Initialize:
   * `minX`, `minY`, `maxX`, `maxY` to compute the bounding rectangle.
   * `totalArea` to sum individual rectangle areas.
   * A `Set` of corner points (represented as strings) to track unpaired corners.

2. For each rectangle `[a, b, c, d]`:
   * Update bounding coordinates.
   * Add its area to `totalArea`.
   * Process all 4 corners: (a,b), (a,d), (c,b), (c,d):
     * If the corner is in the set, remove it (it’s paired).
     * Else, add it (appears odd time so far).

3. After all rectangles:
   * Compare `totalArea` with `(maxX - minX) * (maxY - minY)`.
   * Check that corner set has **exactly 4** points and they match the corners of the bounding rectangle.

4. Return 1 if all conditions met, else 0.

---

# Complexity

* **Time complexity:**
  $O(N)$ for each test case, where $N$ is number of rectangles.

* **Space complexity:**
  $O(K)$ where $K \leq 4 \times N$, for corner tracking.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int perfectRectangle(int[][] rectangles) {
        int minX = Integer.MAX_VALUE, minY = Integer.MAX_VALUE;
        int maxX = Integer.MIN_VALUE, maxY = Integer.MIN_VALUE;
        long totalArea = 0;

        Set<String> cornerSet = new HashSet<>();

        for (int[] rect : rectangles) {
            int a = rect[0], b = rect[1], c = rect[2], d = rect[3];
            minX = Math.min(minX, a);
            minY = Math.min(minY, b);
            maxX = Math.max(maxX, c);
            maxY = Math.max(maxY, d);

            totalArea += (long) (c - a) * (d - b);

            String[] corners = {
                a + "," + b,
                a + "," + d,
                c + "," + b,
                c + "," + d
            };

            for (String corner : corners) {
                if (!cornerSet.add(corner)) {
                    cornerSet.remove(corner);
                }
            }
        }

        // Final area check
        long expectedArea = (long) (maxX - minX) * (maxY - minY);
        if (totalArea != expectedArea) return 0;

        // Only 4 corners should remain and must match the bounding rectangle
        if (cornerSet.size() != 4 ||
            !cornerSet.contains(minX + "," + minY) ||
            !cornerSet.contains(minX + "," + maxY) ||
            !cornerSet.contains(maxX + "," + minY) ||
            !cornerSet.contains(maxX + "," + maxY)) {
            return 0;
        }

        return 1;
    }

    // Optional: for reading input and executing multiple test cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        while (T-- > 0) {
            int N = sc.nextInt();
            int[][] rectangles = new int[N][4];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < 4; j++) {
                    rectangles[i][j] = sc.nextInt();
                }
            }
            System.out.println(perfectRectangle(rectangles));
        }
    }
}
```

---
