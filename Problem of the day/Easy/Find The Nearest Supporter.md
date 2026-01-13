# Intuition

For each contestant, you need the **closest contestant on the left with a strictly smaller rating**.
Brute force checking all left elements for every index is inefficient.
This is a classic **Previous Smaller Element** problem, optimally solved using a **monotonic stack**.

---

# Approach

Maintain a stack that stores ratings in **increasing order from bottom to top**.

Process contestants from left to right:

For each rating `x`:

1. Pop elements from the stack while `stack.top >= x`

   * These cannot be supporters for current or future elements
2. After popping:

   * If stack is empty → no smaller element on the left → store `-1`
   * Else → stack.top is the closest smaller → store its value
3. Push `x` into the stack

**This guarantees:**

* Stack always contains only valid candidates
* The nearest smaller element is always on top

---

# Complexity

- **Time Complexity**
	* `O(N)` per test case
  Each element is pushed and popped at most once.

- **Space Complexity**
	* `O(N)` for the stack

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> prevSmall(ArrayList<Integer> arr, int n) {

        ArrayList<Integer> result = new ArrayList<>();
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < n; i++) {
            int curr = arr.get(i);

            while (!stack.isEmpty() && stack.peek() >= curr) {
                stack.pop();
            }

            if (stack.isEmpty()) {
                result.add(-1);
            } else {
                result.add(stack.peek());
            }

            stack.push(curr);
        }

        return result;
    }
};

```

---

# Example Walkthrough

**Input**

```
[3, 1, 5, 12, 10]
```

**Step-by-step:**

| Index | Value | Stack Before | Action | Output | Stack After |
| ----- | ----- | ------------ | ------ | ------ | ----------- |
| 0     | 3     | []           | empty  | -1     | [3]         |
| 1     | 1     | [3]          | pop 3  | -1     | [1]         |
| 2     | 5     | [1]          | keep   | 1      | [1,5]       |
| 3     | 12    | [1,5]        | keep   | 5      | [1,5,12]    |
| 4     | 10    | [1,5,12]     | pop 12 | 5      | [1,5,10]    |

**Output**

```
[-1, -1, 1, 5, 5]
```

---

## Core insight

- This is not a comparison problem.
- It is a **structure-maintenance problem**.
- The stack enforces correctness by construction.
