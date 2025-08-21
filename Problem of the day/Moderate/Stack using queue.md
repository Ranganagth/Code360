# Intuition

A stack works on the **LIFO (Last-In-First-Out)** principle, while a queue works on **FIFO (First-In-First-Out)**.
To simulate a stack using two queues, we need to ensure that the last pushed element is always dequeued first when performing `pop()` or accessed in `top()`.

One effective way is:

* Always push new elements into an empty queue, then move all elements from the other queue into it.
* Swap the two queues so the main queue always contains the elements in stack order (front = top of stack).

---

# Approach

1. Maintain two queues: `q1` (main queue) and `q2` (helper queue).
2. **push(x)**:

   * Enqueue the new element into `q2`.
   * Move all elements from `q1` into `q2`.
   * Swap `q1` and `q2`. Now `q1` has the updated stack elements in correct order.
3. **pop()**:

   * If `q1` is empty, return `-1`.
   * Otherwise, dequeue from `q1` (this is the stack’s top).
4. **top()**:

   * If `q1` is empty, return `-1`.
   * Otherwise, return the front element of `q1`.
5. **size()**:

   * Return `q1.size()`.
6. **isEmpty()**:

   * Return `q1.isEmpty()`.

This guarantees stack behavior using two queues.

---

# Complexity

* **Time Complexity:**

  * `push(x)` → O(n), because we move all existing elements.
  * `pop()` → O(1).
  * `top()` → O(1).
  * `isEmpty()` and `size()` → O(1).
* **Space Complexity:** O(n), for storing n elements across the two queues.

---

# Code (Java)

```java
import java.util.*;

public class Solution {
    static class Stack {
        private Queue<Integer> q1;
        private Queue<Integer> q2;

        // Constructor
        public Stack() {
            q1 = new LinkedList<>();
            q2 = new LinkedList<>();
        }

        // Returns current size of stack
        public int getSize() {
            return q1.size();
        }

        // Returns true if stack is empty
        public boolean isEmpty() {
            return q1.isEmpty();
        }

        // Push element onto stack
        public void push(int element) {
            // Step 1: Add new element to q2
            q2.add(element);

            // Step 2: Move all elements from q1 to q2
            while (!q1.isEmpty()) {
                q2.add(q1.poll());
            }

            // Step 3: Swap references of q1 and q2
            Queue<Integer> temp = q1;
            q1 = q2;
            q2 = temp;
        }

        // Pop element from stack
        public int pop() {
            if (q1.isEmpty()) return -1;
            return q1.poll();
        }

        // Return top element
        public int top() {
            if (q1.isEmpty()) return -1;
            return q1.peek();
        }
    }

    // Sample driver
    public static void main(String[] args) {
        Stack st = new Stack();
        
        // Example Test Case
        st.push(13);
        st.push(47);
        System.out.println(st.getSize());   // Output: 2
        System.out.println(st.isEmpty());   // Output: false
        System.out.println(st.pop());       // Output: 47
        System.out.println(st.top());       // Output: 13
    }
}
```

---

# Example Walkthrough

**Input:**

```
6
1 13
1 47
4
5
2
3
```

**Execution:**

1. `1 13` → push(13). Stack = \[13]
2. `1 47` → push(47). Stack = \[47, 13]
3. `4` → size() → 2
4. `5` → isEmpty() → false
5. `2` → pop() → removes 47 → output `47`. Stack = \[13]
6. `3` → top() → `13`

**Output:**

```
2
false
47
13
```

---

