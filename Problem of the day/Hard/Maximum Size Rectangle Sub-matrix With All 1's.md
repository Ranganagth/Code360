# Intuition

Convert each row into a histogram of consecutive 1’s height.
For every row, treat it as the base of a histogram and compute the **largest rectangle area** using the classic “largest rectangle in histogram” stack method.
The maximum across all rows is the answer.

# Approach

1. Maintain a height array of size M.
2. For each row:

   * If `mat[i][j] == 1`, increment `height[j]`.
   * Else set `height[j] = 0`.
3. For each updated height array, compute largest rectangle:

   * Use a monotonic stack storing indices.
   * Whenever current height < height[stack top], pop and compute rectangle for popped bar.
   * Area = height[popped] * width.
4. Track the global maximum.

# Complexity

* Time complexity:
  **O(N * M)** for building histograms
  **+ O(N * M)** for histogram largest-rectangle calculations
  Overall: **O(N * M)**
* Space complexity:
  **O(M)** for height[] and stack

# Code

```java
public class Solution {
    public static int maximalAreaOfSubMatrixOfAll1(int[][] mat, int n, int m) {

        int[] height = new int[m];
        int maxArea = 0;

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < m; j++) {
                if (mat[i][j] == 1) height[j] += 1;
                else height[j] = 0;
            }

            maxArea = Math.max(maxArea, largestRectangle(height));
        }

        return maxArea;
    }

    private static int largestRectangle(int[] heights) {
        int n = heights.length;
        int maxArea = 0;

        java.util.Stack<Integer> st = new java.util.Stack<>();

        for (int i = 0; i <= n; i++) {
            int curr = (i == n) ? 0 : heights[i];

            while (!st.isEmpty() && curr < heights[st.peek()]) {
                int h = heights[st.pop()];
                int r = i;
                int l = st.isEmpty() ? 0 : st.peek() + 1;
                maxArea = Math.max(maxArea, h * (r - l));
            }
            st.push(i);
        }

        return maxArea;
    }
}
```

# Example Walkthrough

Matrix:

```
1 0 1 1
1 0 1 1
0 1 0 1
1 1 1 1
0 0 0 1
```

### Row 1 heights:

`1 0 1 1` → max rectangle = 2 (1×2)

### Row 2 heights:

`2 0 2 2` → max rectangle = 4 (2×2)

### Row 3 heights:

`0 1 0 3` → max rectangle = 3 (3×1)

### Row 4 heights:

`1 2 1 4` → max rectangle = 5 (height=1, width=4 → 4; height=2, width=2 → 4; height=4, width=1 → 4; max=5)

### Row 5 heights:

`0 0 0 5` → max rectangle = 5

Final answer = **5**
