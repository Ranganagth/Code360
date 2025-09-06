# Intuition

* A triangle is formed by **3 points**.
* Its area can be computed using the **shoelace formula / determinant method**:

$$
\text{Area} = \frac{1}{2} \times |x_1(y_2 - y_3) + x_2(y_3 - y_1) + x_3(y_1 - y_2)|
$$

* To get the **maximum area**, we need to check all possible combinations of 3 points and keep track of the maximum.

---

# Approach

1. Iterate over all triplets of points `(i, j, k)` where `i < j < k`.
2. For each triplet, calculate the area using the determinant formula.
3. Update the maximum if this area is larger.
4. Return the maximum area (formatted to 5 decimal places as per sample output).

---

# Complexity

* There are at most **N = 100 points**.
* Number of triplets = $\binom{N}{3} = O(N^3)$.
* $100^3 = 1,000,000$ iterations → feasible within 1 second.
* Each area calculation is **O(1)**.
* Total complexity: **O(N^3)** time, **O(1)** space.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static double calculateArea(int n, int points[][]) {
        double maxArea = 0.0;

        // Try all triplets
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    int x1 = points[i][0], y1 = points[i][1];
                    int x2 = points[j][0], y2 = points[j][1];
                    int x3 = points[k][0], y3 = points[k][1];

                    // Shoelace / determinant formula
                    double area = Math.abs(
                        x1 * (y2 - y3) +
                        x2 * (y3 - y1) +
                        x3 * (y1 - y2)
                    ) / 2.0;

                    maxArea = Math.max(maxArea, area);
                }
            }
        }

        return maxArea;
    }

    // For multiple test cases
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();

        while (T-- > 0) {
            int n = sc.nextInt();
            int[][] points = new int[n][2];
            for (int i = 0; i < n; i++) {
                points[i][0] = sc.nextInt();
                points[i][1] = sc.nextInt();
            }

            double ans = calculateArea(n, points);
            System.out.printf("%.5f\n", ans);
        }
    }
}
```

---

## Example Walkthrough

Input:

```
5
0 0
0 1
1 0
0 2
2 0
```

Triplets to check:

* (0,0), (0,2), (2,0) → area = 2
* (0,1), (0,2), (2,0) → area smaller
* (0,0), (1,0), (0,2) → area smaller
* …
  Maximum = **2.00000**

---
