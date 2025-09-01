# Intuition

We need to simulate a queue (**FIFO**) using two stacks (**LIFO**).

* Use **two stacks**:

  * `stk1` → for enqueue (push new elements).
  * `stk2` → for dequeue (pop oldest elements).

**Logic:**

* `enqueue(x)` → Just push into `stk1`.
* `dequeue()` →

  * If `stk2` is empty, move all elements from `stk1` to `stk2` (this reverses order).
  * Pop from `stk2`.
  * If both are empty → return `-1`.

This works because moving from `stk1 → stk2` ensures that the oldest element ends up on top of `stk2`.

---

# Approach

1. Maintain two stacks `stk1` and `stk2`.
2. For `enqueue(x)`: push `x` to `stk1`. Return `true`.
3. For `dequeue()`:

   * If both stacks are empty → return `-1`.
   * If `stk2` is empty → move all elements from `stk1` to `stk2`.
   * Pop from `stk2` and return that element.

---

# Complexity

* **Amortized Time Complexity**:
  * `enqueue`: `O(1)`
  * `dequeue`: `O(1)` amortized (since each element moves at most once from `stk1` → `stk2`).
    
* **Space Complexity**: `O(N)` (for storing elements).

---

# Code

```java
import java.util.*;
import java.io.*;

class Queue {
    // Stacks to be used in the operations.
    Stack<Integer> stk1, stk2;

    public Queue() {
        // Initialize stacks
        stk1 = new Stack<>();
        stk2 = new Stack<>();
    }

    // Enqueues 'x' into the queue. Returns true after enqueuing.
    public boolean enqueue(int x) {
        stk1.push(x);
        return true;
    }

    /*
       Dequeues top element from queue. Returns -1 if the queue is empty, 
       otherwise returns the popped element.
    */
    public int dequeue() {
        if (stk1.isEmpty() && stk2.isEmpty()) {
            return -1; // Queue is empty
        }

        // If stk2 is empty, move all elements from stk1 to stk2
        if (stk2.isEmpty()) {
            while (!stk1.isEmpty()) {
                stk2.push(stk1.pop());
            }
        }

        return stk2.pop();
    }
}
```

---

## Walkthrough with Example

Input:

```
7
1 2 
1 3 
2 
1 4 
1 6 
1 7 
2
```

* `enqueue(2)` → queue = \[2] → return `True`.
* `enqueue(3)` → queue = \[2, 3] → return `True`.
* `dequeue()` → returns 2.
* `enqueue(4)` → queue = \[3, 4] → return `True`.
* `enqueue(6)` → queue = \[3, 4, 6] → return `True`.
* `enqueue(7)` → queue = \[3, 4, 6, 7] → return `True`.
* `dequeue()` → returns 3.

Output:

```
True
True
2
True
True
True
3
```

---
