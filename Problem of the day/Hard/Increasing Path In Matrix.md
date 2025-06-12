# Intuition

For each bar in the histogram, imagine that the bar is the shortest one in a rectangle. We find how far this rectangle can extend to the left and right. This can be efficiently done using a stack.

# Approach

1. Append a zero to the end of the array to make sure all rectangles are computed (forces remaining items out of the stack).

2. Traverse the `heights` array.

3. Use a stack to store **indices** of the histogram bars in increasing order.

4. For every bar:
   * While the current bar is **less** than the height at the index on top of the stack:
     * Pop from the stack and calculate the rectangle area:
       * Height = `heights[stack.pop()]`
       * Width = `i - stack.peek() - 1` (if stack is not empty), else `i`
     * Update the max area.
	 
5. Return the maximum area found.

# Complexity Analysis

* **Time:** `O(N)` per test case (each bar is pushed and popped once)
* **Space:** `O(N)` for the stack

# Code

```java
import java.util.*;

public class Solution {
    public static int largestRectangle(ArrayList<Integer> heights) {
        int n = heights.size();
        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;

        // Append a zero-height bar at the end to flush remaining items
        heights.add(0);

        for (int i = 0; i < heights.size(); i++) {
            while (!stack.isEmpty() && heights.get(i) < heights.get(stack.peek())) {
                int height = heights.get(stack.pop());
                int width;
                if (stack.isEmpty()) {
                    width = i;
                } else {
                    width = i - stack.peek() - 1;
                }
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }

        return maxArea;
    }
}
```

---

### Example

For the input:
`[2, 1, 5, 6, 2, 3]`

The largest rectangle is formed between indices `2` and `3`: `min(5,6) * 2 = 10`
