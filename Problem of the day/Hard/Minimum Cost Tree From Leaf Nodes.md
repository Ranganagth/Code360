# Intuition:

* To **minimize** the total cost (sum of non-leaf nodes), we want to **pair the smallest leaves early**.
* This resembles the **"Minimum Cost Tree from Leaf Values"** problem solved with a **greedy monotonic decreasing stack**.

#### **Key Idea:**

For each element:
* Keep popping from the stack while top of the stack is **less than or equal to** current element.
* Each pop means we are building a tree with that element and the **smaller of its neighbors** to keep the cost minimum.

---

# Complexity Analysis

* **Time**: `O(N)`
  Every element is pushed and popped **at most once**
* **Space**: `O(N)` for the stack

---

# Code:

```java
import java.util.*;

public class Solution {
    public static int minimumCostTreeFromLeafNodes(ArrayList<Integer> arr) {
        Stack<Integer> stack = new Stack<>();
        int totalCost = 0;

        for (int num : arr) {
            while (!stack.isEmpty() && stack.peek() <= num) {
                int mid = stack.pop();
                if (!stack.isEmpty()) {
                    totalCost += mid * Math.min(stack.peek(), num);
                } else {
                    totalCost += mid * num;
                }
            }
            stack.push(num);
        }

        // Process remaining elements in stack
        while (stack.size() >= 2) {
            int top = stack.pop();
            totalCost += top * stack.peek();
        }

        return totalCost;
    }
}
```

---

### **Example Walkthrough**

**Input**: `[1, 2, 3, 4]`
**Steps**:
* Stack: `[1]` → compare `2`, pop `1`, cost += `1*2 = 2`
* Stack: `[2]` → compare `3`, pop `2`, cost += `2*3 = 6`
* Stack: `[3]` → compare `4`, pop `3`, cost += `3*4 = 12`
* Final stack: `[4]` → done

**Total** = `2 + 6 + 12 = 20`

Matches the expected result!

---

### **Summary:**

* Always use this optimized greedy method unless problem **specifically** demands tree construction or intermediate steps.
* Handles `n ≤ 40` effortlessly and is **substantially faster** than `O(n^3)` DP.

---