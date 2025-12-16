# Intuition

For every contiguous window of size `K`, the task is to report the **first negative number** appearing in that window.
Brute force scanning each window costs `O(N*K)` and is unnecessary.
The correct abstraction is to **track only indices of negative numbers** that are still inside the current window.

---

# Approach

Use a **deque (double-ended queue)** to store indices of negative elements.

Process the array from left to right:

1. **While adding a new element (index `i`)**

   * If `arr[i]` is negative, push its index into the deque.

2. **Before processing a window ending at `i`**

   * Remove indices from the front of the deque that are **out of the current window** (`index < i - k + 1`).

3. **Once the first window is formed (`i >= k - 1`)**

   * If the deque is not empty → the front index gives the first negative number.
   * If empty → no negative in this window, output `0`.

Each index enters and leaves the deque exactly once.

---

# Complexity

* **Time Complexity:** `O(N)` per test case
* **Space Complexity:** `O(K)` for the deque

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> firstNegativeInteger(ArrayList<Integer> arr, int k, int n) {

        ArrayList<Integer> result = new ArrayList<>();
        Deque<Integer> dq = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {

            if (arr.get(i) < 0) {
                dq.addLast(i);
            }

            while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) {
                dq.pollFirst();
            }

            if (i >= k - 1) {
                if (dq.isEmpty()) {
                    result.add(0);
                } else {
                    result.add(arr.get(dq.peekFirst()));
                }
            }
        }

        return result;
    }
};

```

---

# Example Walkthrough

**Input**

```
arr = [-10, 20, -30, -40, 50, 60, -70, 80, 90]
k = 3
```

**Window-wise evaluation**

```
[-10, 20, -30]  → -10
[20, -30, -40]  → -30
[-30, -40, 50]  → -30
[-40, 50, 60]   → -40
[50, 60, -70]   → -70
[60, -70, 80]   → -70
[-70, 80, 90]   → -70
```

**Output**

```
[-10, -30, -30, -40, -70, -70, -70]
```

---

