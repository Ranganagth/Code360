# Intuition

If multiple points lie on the same straight line, the **slope between any pair of those points** will be the same. We can compute the slope between a fixed point and all others and keep track of how many share the same slope.

To avoid precision issues with floating-point division, we use a **reduced fraction** `(dy, dx)` as the key for slopes by using GCD.

---

# Approach

1. For every point `i`:

   * Initialize a `HashMap` to store frequency of each unique slope from point `i` to others.
   * For each other point `j`:

     * If `i == j`, continue.
     * Compute `dx = xj - xi`, `dy = yj - yi`.
     * Reduce the slope to simplest form using GCD.
     * Track slope as `(dy/gcd, dx/gcd)` as key in the map.
     * Also, track duplicate points.
   * After checking all points from `i`, compute the maximum number of points with the most common slope + duplicates + 1 (self).
2. Return the global maximum.

---

# Complexity

* **Time complexity**: $O(N^2)$ per test case
* **Space complexity**: $O(N)$ for storing slopes per iteration

This is efficient for $N \leq 10^3$.

---

# Code

```java
import java.util.*;

public class Solution {
	public static int maxPointsOnLine(int[][] points, int n) {
		if (n <= 2) return n;
		int maxPoints = 0;

		for (int i = 0; i < n; i++) {
			Map<String, Integer> slopeCount = new HashMap<>();
			int duplicates = 0;
			int curMax = 0;

			for (int j = 0; j < n; j++) {
				if (i == j) continue;

				int dx = points[j][0] - points[i][0];
				int dy = points[j][1] - points[i][1];

				if (dx == 0 && dy == 0) {
					duplicates++;
					continue;
				}

				int gcd = gcd(dy, dx);
				dx /= gcd;
				dy /= gcd;

				// Normalize to avoid -1/2 vs 1/-2 confusion
				if (dx < 0) {
					dx = -dx;
					dy = -dy;
				}

				String slopeKey = dy + "/" + dx;
				slopeCount.put(slopeKey, slopeCount.getOrDefault(slopeKey, 0) + 1);
				curMax = Math.max(curMax, slopeCount.get(slopeKey));
			}

			maxPoints = Math.max(maxPoints, curMax + duplicates + 1); // +1 for the point itself
		}

		return maxPoints;
	}

	private static int gcd(int a, int b) {
		if (b == 0) return a;
		return gcd(b, a % b);
	}
}
```

---

