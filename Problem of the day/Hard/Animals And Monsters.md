> Partial acceptance - Passed 10/11 test cases
# Intuition

To solve this problem, you need to **check if a point lies inside a polygon** defined by a list of its vertices. A well-known and efficient approach to do this is the **Ray Casting Algorithm** (also known as the even–odd rule algorithm).

---

# Approach

1. Draw a horizontal ray to the right from the point (animal's location).
2. Count how many times this ray intersects with the edges of the polygon (fence).
3. If the number of intersections is **odd**, the point is **inside**.
4. If **even**, the point is **outside**.
5. If the point lies **on an edge**, it is considered **inside**.

### Steps

* Loop through each edge of the polygon: from point `i` to point `j = (i + 1) % n`.
* Check if the ray crosses this edge.
* Handle boundary conditions carefully, especially when the point lies **on a segment**.

---

# Complexity Analysis

* Each test case: O(N) where N ≤ 2000
* Efficient and acceptable within constraints.

---

# Code

```java
public class Solution {
    public static int isSafe(int n, int[][] points, int x, int y) {
        if (n < 3) return 0; // A polygon must have at least 3 sides

        boolean inside = false;

        for (int i = 0, j = n - 1; i < n; j = i++) {
            int xi = points[i][0], yi = points[i][1];
            int xj = points[j][0], yj = points[j][1];

            // Check if point lies exactly on an edge
            if (onSegment(xi, yi, xj, yj, x, y)) return 1;

            // Check for ray intersection
            boolean intersect = ((yi > y) != (yj > y)) &&
                    (x < (long)(xj - xi) * (y - yi) / (yj - yi + 0.0) + xi);
            if (intersect) inside = !inside;
        }

        return inside ? 1 : 0;
    }

    private static boolean onSegment(int xi, int yi, int xj, int yj, int x, int y) {
        // Check if (x, y) lies on the line segment from (xi, yi) to (xj, yj)
        long cross = (long)(x - xi) * (yj - yi) - (long)(y - yi) * (xj - xi);
        if (cross != 0) return false;

        // Check if (x, y) is within the bounding box of the segment
        return Math.min(xi, xj) <= x && x <= Math.max(xi, xj)
            && Math.min(yi, yj) <= y && y <= Math.max(yi, yj);
    }
}
```

---

### **Example**

For input:

```
1
3
0 0
5 0
5 5
3 3
```

* The triangle is formed by (0,0), (5,0), (5,5)
* Animal at (3,3) is **inside** → Output: `1`

---
