# Intuition

Interleave by isolating the first half, then alternately push from first-half and the remaining queue. Queue properties handle order without extra restructuring.

---

# Approach

Use a stack to reverse the first half into correct dequeue order.

1. Push first N/2 elements into a stack.
2. Pop all from stack back into queue.
3. Move first N/2 elements (the reversed first half) to the back, restoring original order.
4. Again push first N/2 elements into a stack.
5. Now interleave: for each element popped from stack, push it to queue then immediately push the next element from queue.

---

# Complexity

- **Time:** O(N).
- **Space:** O(N).

---
# Code

```java
import java.util.*;

public class Solution {
    public static void interLeaveQueue(Queue<Integer> q) {
        int n = q.size();
        int half = n / 2;
        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < half; i++) st.push(q.remove());
        while (!st.isEmpty()) q.add(st.pop());

        for (int i = 0; i < half; i++) q.add(q.remove());

        for (int i = 0; i < half; i++) st.push(q.remove());

        while (!st.isEmpty()) {
            q.add(st.pop());
            q.add(q.remove());
        }
    }
};

```

---

# Example walkthrough

**Input queue:**
[10, 20, 30, 40, 50, 60]

**Step 1:** push first half to stack → stack: [10, 20, 30]
Queue: [40, 50, 60]

**Step 2:** pop stack to queue
Queue: [40, 50, 60, 30, 20, 10]

**Step 3:** rotate first half
Queue: [30, 20, 10, 40, 50, 60]

**Step 4:** push first half to stack
Stack: [30, 20, 10]
Queue: [40, 50, 60]

**Step 5:** interleave
pop 30 → queue: [40, 50, 60, 30], then push 40 → [50, 60, 30, 40]
pop 20 → queue: [50, 60, 30, 40, 20], then push 50 → [60, 30, 40, 20, 50]
pop 10 → queue: [60, 30, 40, 20, 50, 10], then push 60 → [30, 40, 20, 50, 10, 60]

**Final queue:**
[10, 40, 20, 50, 30, 60]
