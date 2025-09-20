# Intuition

* We have `N` questions labeled `1…N`.
* Starting from question `1`, Ninja skips one and solves the next.
* This is equivalent to: remove every 2nd element in a circular list until empty.
* The order in which elements are removed is the answer.

---

# Approach

1. Initialize a queue (or list) with all numbers `1…N`.
2. Use a counter:

   * Pop the front element, and keep rotating.
   * Skip one (put it back at the end).
   * Solve the next (remove it, add to result).
3. Continue until all elements are solved.
4. Complexity is `O(N)` per test case if we simulate with a queue efficiently.

---

# Complexity

* **Time:** `O(N)` (each element is enqueued/dequeued at most once).
* **Space:** `O(N)` (queue + result list).

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> theOrder(int n) {
        ArrayList<Integer> result = new ArrayList<>();
        Queue<Integer> q = new LinkedList<>();

        // Fill queue with 1..n
        for (int i = 1; i <= n; i++) {
            q.offer(i);
        }

        while (!q.isEmpty()) {
            // Step 1: skip one (rotate front to back)
            if (!q.isEmpty()) {
                q.offer(q.poll());
            }

            // Step 2: solve next (remove and add to result)
            if (!q.isEmpty()) {
                result.add(q.poll());
            }
        }

        return result;
    }
}
```

---

## Walkthrough Example

Input: `N = 5`
Queue initially: `[1, 2, 3, 4, 5]`

* Skip 1 → move `1` to back → `[2, 3, 4, 5, 1]`

* Solve 2 → remove `2` → result = `[2]`

* Skip 3 → `[4, 5, 1, 3]`

* Solve 4 → result = `[2, 4]`

* Skip 5 → `[1, 3, 5]`

* Solve 1 → result = `[2, 4, 1]`

* Skip 3 → `[5, 3]`

* Solve 5 → result = `[2, 4, 1, 5]`

* Solve last 3 → result = `[2, 4, 1, 5, 3]`.

Matches expected.

---

