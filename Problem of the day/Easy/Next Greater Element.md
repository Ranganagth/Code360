# Intuition

We want to find, for each element, the **first larger element to its right**. If we iterate from **right to left** and keep track of a stack of potential "next greater" elements, we can do this efficiently in linear time.

---

# Approach

1. Initialize an empty stack and an array `res` of size `n` to store answers.
2. Traverse the array from the **end to the beginning**:

   * While the stack is not empty and the top of the stack is **less than or equal to** the current element, pop it.
   * If the stack is empty after popping, there is no next greater element: set result as `-1`.
   * Otherwise, the top of the stack is the next greater element.
   * Push the current element onto the stack for future comparisons.
3. Return the result list.

---

# Complexity

* **Time complexity**: $O(n)$ for each test case
  Each element is pushed and popped at most once from the stack.
* **Space complexity**: $O(n)$
  For the result list and the auxiliary stack.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> nextGreaterElement(ArrayList<Integer> arr, int n) {
        ArrayList<Integer> result = new ArrayList<>(Collections.nCopies(n, -1));
        Stack<Integer> stack = new Stack<>();

        for (int i = n - 1; i >= 0; i--) {
            int current = arr.get(i);

            // Remove elements that are not greater
            while (!stack.isEmpty() && stack.peek() <= current) {
                stack.pop();
            }

            // If stack is not empty, the top is the next greater
            if (!stack.isEmpty()) {
                result.set(i, stack.peek());
            }

            // Push current element to stack
            stack.push(current);
        }

        return result;
    }
}
```

---
