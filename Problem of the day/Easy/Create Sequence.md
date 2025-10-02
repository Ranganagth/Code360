# Intuition

We need to generate all numbers less than `N` that consist **only of digits `2` and `5`**.
This is like generating numbers in base-2 but with digits `{2, 5}` instead of `{0, 1}`.

Think of it as a **tree generation problem**:

* Start with `2` and `5`
* From each number, append another digit `2` or `5` at the end (like BFS/DFS generation).
* Stop if the number exceeds `N`.

This ensures we generate all possible valid numbers.

---

# Approach

1. Use a **queue (BFS)** to generate numbers level by level.

   * Start with `[2, 5]`.
   * Pop a number `curr`, add it to result if `curr < N`.
   * Generate `curr * 10 + 2` and `curr * 10 + 5`, and push them back into the queue if they are `< N`.
2. Keep generating until the queue is empty.
3. Sort the result because BFS might not always give ascending order.

---

# Complexity

* Each valid number less than `N` is generated **once**.
* Suppose there are `k` such numbers:

  * **Time:** `O(k log k)` (due to sorting at the end).
  * **Space:** `O(k)` for storing results and queue.

Since `N` can be as large as `10^16`, the number of valid numbers grows but is manageable because digits are restricted to `{2, 5}` only.

---
# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Long> createSequence(long n) {
        ArrayList<Long> result = new ArrayList<>();
        Queue<Long> q = new LinkedList<>();
        
        // Start with 2 and 5
        q.add(2L);
        q.add(5L);
        
        while (!q.isEmpty()) {
            long curr = q.poll();
            if (curr <= n) {
                result.add(curr);
                
                // Generate next numbers
                q.add(curr * 10 + 2);
                q.add(curr * 10 + 5);
            }
        }
        
        Collections.sort(result); // ensure ascending order
        return result;
    }
};

```

---

## Example Walkthrough

### Input:

```
N = 30
```

### Steps:

* Start: queue = [2, 5]
* Pop 2 → result = [2], push 22, 25
* Pop 5 → result = [2, 5], push 52, 55 (ignored later since > 30)
* Pop 22 → result = [2, 5, 22], push 222, 225 (> 30 ignored)
* Pop 25 → result = [2, 5, 22, 25]
* Queue ends

Final result: `[2, 5, 22, 25]`

---

