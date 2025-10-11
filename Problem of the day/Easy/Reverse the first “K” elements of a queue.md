# Intuition

We are given a **queue** and need to **reverse the first `k` elements**, while **keeping the rest of the elements in their original order**.

For example:
```
Queue: [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
k = 4
Output: [40, 30, 20, 10, 50, 60, 70, 80, 90, 100]
```

So only the **first 4 elements** are reversed, and everything after that remains as is.

---

# Approach

We can solve this problem efficiently using a **stack** and **queue operations**.

### Steps:

1. **Push the first k elements into a stack**  
   - This will reverse their order because stacks are LIFO (Last-In-First-Out).

2. **Pop all elements from the stack and enqueue back into the queue**  
   - Now the first `k` elements are reversed and at the back of the queue.

3. **Move the remaining (n - k) elements from the front to the back**  
   - This step restores the original relative order of the remaining elements.

---

# Complexity

| Operation | Time Complexity | Space Complexity |
|------------|----------------|------------------|
| Reversal using stack | O(K) | O(K) |
| Rotating queue | O(N - K) | O(1) |
| **Total** | **O(N)** | **O(K)** |

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static void reverse(Queue<Integer> queue, int k) {
        if (queue == null || k <= 0 || k > queue.size()) return;

        Stack<Integer> stack = new Stack<>();

        // Step 1: Push first k elements into stack
        for (int i = 0; i < k; i++) {
            stack.push(queue.poll());
        }

        // Step 2: Enqueue back reversed elements
        while (!stack.isEmpty()) {
            queue.add(stack.pop());
        }

        // Step 3: Move remaining elements (n - k) to back
        int remaining = queue.size() - k;
        for (int i = 0; i < remaining; i++) {
            queue.add(queue.poll());
        }
    }
};

```

---

## Example Execution

**Input:**
```
10
10 20 30 40 50 60 70 80 90 100
4
```

**Output:**
```
40 30 20 10 50 60 70 80 90 100
```


For `queue = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]` and `k = 4`

**Step 1:**  
Push first 4 elements into a stack  
Stack = [10, 20, 30, 40]

**Step 2:**  
Pop from stack and enqueue → Queue = [50, 60, 70, 80, 90, 100, 40, 30, 20, 10]

**Step 3:**  
Move (n - k = 6) elements from front to back:
After rotation → Queue = [40, 30, 20, 10, 50, 60, 70, 80, 90, 100]

Result achieved.

