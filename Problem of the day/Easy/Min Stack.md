# Intuition

A normal stack cannot tell the minimum in O(1) because popping might remove the current minimum and then the next minimum is unknown. The fix: store extra information so every stack state already “knows” its minimum. Then every push/pop simply moves between states.

---

# Approach

Maintain two stacks (or one stack of pairs).
Primary stack → actual elements
Min stack → minimum seen up to each position

**Push(x):**
• Push x on primary stack.
• Push min(x, currentMin) on min stack.

**Pop():**
• If empty → return -1
• Pop from both stacks and return popped element.

**Top():**
• If empty → return -1
• Return top of primary stack.

**getMin():**
• If empty → return -1
• Return top of min stack.

Each operation runs in constant time because at most one push/pop happens per stack.

---

# Complexity

- **Time:** O(1) per operation
- **Space:** O(n) for storing elements and minimums

---

# Code

```java
import java.util.*;

public class Solution {

    static class MinStack {

        private Stack<Integer> st;
        private Stack<Integer> minSt;

        MinStack() {
            st = new Stack<>();
            minSt = new Stack<>();
        }

        void push(int num) {
            st.push(num);
            if (minSt.isEmpty()) {
                minSt.push(num);
            } else {
                minSt.push(Math.min(num, minSt.peek()));
            }
        }

        int pop() {
            if (st.isEmpty()) {
                return -1;
            }
            minSt.pop();
            return st.pop();
        }

        int top() {
            if (st.isEmpty()) {
                return -1;
            }
            return st.peek();
        }

        int getMin() {
            if (minSt.isEmpty()) {
                return -1;
            }
            return minSt.peek();
        }
    }
}
```

---

# Example walkthrough

**Operations:**
1 1   → push(1)
1 2   → push(2)
4     → getMin()
2     → pop()
3     → top()

**State after pushes:**
Primary stack: [1, 2]
Min stack:      [1, 1]

**getMin():** top(min stack) = 1

**pop():** removes 2
Primary stack: [1]
Min stack:      [1]

**top():** returns 1

**Output:**
1 2 1

---
