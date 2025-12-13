# Intuition

A queue is FIFO. Reversal requires last element to come first. Using only queue operations, direct reversal is impossible. A stack provides LIFO behavior, which exactly inverts order when elements are transferred back.

---

# Approach

1. Remove elements from the queue one by one using `dequeue()` and push them onto a stack.  
2. Pop elements from the stack and insert them back into the queue using `enqueue()`.  
3. The queue order is now reversed.

---

# Complexity

- Time complexity: **O(N)**  
- Space complexity: **O(N)** (auxiliary stack)

---

# Code

```java
import java.util.*;

public class Solution {

    public static Queue<Integer> reverseQueue(Queue<Integer> q) {

        Stack<Integer> stack = new Stack<>();

        while (!q.isEmpty()) {
            stack.push(q.remove());
        }

        while (!stack.isEmpty()) {
            q.add(stack.pop());
        }

        return q;
    }
};

```

---

# Example Walkthrough

Input Queue: `[10, 6, 8, 12, 3]`

Step 1 – Move to stack:  
Stack (top → bottom): `3, 12, 8, 6, 10`  
Queue becomes empty.

Step 2 – Move back to queue:  
Queue after reinsert: `[3, 12, 8, 6, 10]`

**Final reversed queue obtained.**