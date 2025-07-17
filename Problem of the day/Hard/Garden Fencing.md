# Intuition

We need to find the **smallest circle** that encloses all the given points (trees), either **inside** or **on the boundary**. This is a classic **computational geometry** problem known as **"Smallest Enclosing Circle"** or **"Minimum Bounding Circle"**.

---

# Approach

We'll use **Welzl's algorithm**, a recursive and randomized algorithm to find the **minimum enclosing circle** in **expected linear time**.

#### Steps:
1. **Shuffle the points** to ensure random order (important for expected performance).
2. Start with an **empty boundary** and recursively try to build the smallest circle that includes the current point.
3. If a point lies **outside** the current circle, it's added to the boundary (circle must pass through it), and the circle is recalculated.
4. The boundary has at most **3 points** (since a circle is uniquely defined by 3 non-collinear points).

---

# Complexity Analysis

* **Time Complexity**: `O(N)` expected, `O(N^3)` worst-case due to combinatorics in boundary handling (3 points).
* **Space Complexity**: `O(N)` for storing the tree coordinates.

---

# Code

```java
import java.util.*;

public class Solution {

    static class Point {
        double x, y;

        Point(int[] coord) {
            this.x = coord[0];
            this.y = coord[1];
        }

        Point(double x, double y) {
            this.x = x;
            this.y = y;
        }

        double dist(Point p) {
            return Math.hypot(x - p.x, y - p.y);
        }
    }

    static class Circle {
        Point c;
        double r;

        Circle(Point c, double r) {
            this.c = c;
            this.r = r;
        }

        boolean contains(Point p) {
            return c.dist(p) <= r + 1e-10;
        }
    }

    public static double[] gardenFencing(int n, int[][] trees) {
        List<Point> points = new ArrayList<>();
        for (int[] tree : trees) points.add(new Point(tree));
        Circle circle = minCircle(points);
        return new double[]{circle.c.x, circle.c.y, circle.r};
    }

    private static Circle minCircle(List<Point> points) {
        Collections.shuffle(points, new Random());
        return welzl(points, new ArrayList<>(), points.size());
    }

    private static Circle welzl(List<Point> points, List<Point> boundary, int n) {
        if (n == 0 || boundary.size() == 3) {
            return trivialCircle(boundary);
        }

        Point p = points.get(n - 1);
        Circle circle = welzl(points, boundary, n - 1);

        if (circle.contains(p)) return circle;

        boundary.add(p);
        Circle res = welzl(points, boundary, n - 1);
        boundary.remove(boundary.size() - 1);
        return res;
    }

    private static Circle trivialCircle(List<Point> points) {
        if (points.isEmpty()) return new Circle(new Point(0, 0), 0);
        if (points.size() == 1) return new Circle(points.get(0), 0);
        if (points.size() == 2) return makeDiameter(points.get(0), points.get(1));
        return circleFrom3(points.get(0), points.get(1), points.get(2));
    }

    private static Circle makeDiameter(Point a, Point b) {
        Point center = new Point((a.x + b.x) / 2.0, (a.y + b.y) / 2.0);
        double radius = center.dist(a);
        return new Circle(center, radius);
    }

    private static Circle circleFrom3(Point a, Point b, Point c) {
        double A = b.x - a.x;
        double B = b.y - a.y;
        double C = c.x - a.x;
        double D = c.y - a.y;
        double E = A * (a.x + b.x) + B * (a.y + b.y);
        double F = C * (a.x + c.x) + D * (a.y + c.y);
        double G = 2.0 * (A * (c.y - b.y) - B * (c.x - b.x));

        if (G == 0) {
            return new Circle(new Point(0, 0), Double.MAX_VALUE); // Degenerate
        }

        double cx = (D * E - B * F) / G;
        double cy = (A * F - C * E) / G;
        Point center = new Point(cx, cy);
        double radius = center.dist(a);
        return new Circle(center, radius);
    }
}
```

---

### **Example Walkthrough**

#### Input:

```
3
(0, 0), (1, 0), (2, 0)
```

* Smallest enclosing circle is formed by using (0, 0) and (2, 0) as endpoints.
* Center = (1, 0)
* Radius = 1

#### Output:

```
1.0 0.0 1.0
```

#### Input:

```
4
(1, 0), (0, -1), (-1, 0), (0, 1)
```

* Circle centered at origin (0, 0)
* All 4 points lie on unit circle
* Radius = 1

#### Output:

```
0.0 0.0 1.0
```

---
