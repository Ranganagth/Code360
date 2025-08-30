# Intuition

* We are given:

  * `manager[i]`: direct manager of employee `i`.
  * `timeToInform[i]`: time taken for employee `i` to inform *all* of their subordinates.
  * `headId`: root of the tree (company head).

* The hierarchy forms a **tree**:

  * Each employee has **at most one manager**.
  * Employees can have many subordinates.

* The total time to spread info:

  * `time[head] + max(time[subordinates])`
  * Because the head informs all subordinates in parallel, so we take the **maximum path length**.

---

# Approach

1. Build adjacency list: For each manager, collect their subordinates.
2. DFS from `headId`:

   * If employee has no subordinates → return 0.
   * Otherwise:

     * Recursively compute time for each subordinate.
     * Result = `timeToInform[current] + max(subordinateTimes)`.
3. Return the result.

---
# Complexity

* Building tree: **O(N)**
* DFS traversal: **O(N)**
* Total: **O(N)**, which works for `N ≤ 10^5`.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int informEmployees(int manager[], int timeToInform[], int headId) {
        int n = manager.length;
        // Build tree (subordinates list)
        List<List<Integer>> subordinates = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            subordinates.add(new ArrayList<>());
        }
        for (int i = 0; i < n; i++) {
            if (manager[i] != -1) {
                subordinates.get(manager[i]).add(i);
            }
        }

        // DFS function
        return dfs(headId, subordinates, timeToInform);
    }

    private static int dfs(int curr, List<List<Integer>> subordinates, int[] timeToInform) {
        int maxTime = 0;
        for (int sub : subordinates.get(curr)) {
            maxTime = Math.max(maxTime, dfs(sub, subordinates, timeToInform));
        }
        return timeToInform[curr] + maxTime;
    }
}
```

---

## **Example Walkthrough**

### Example:

`manager = [-1, 0, 0, 1, 1]`,
`timeToInform = [1, 1, 0, 0, 0]`,
`headId = 0`.

* Build tree:

  * 0 → \[1, 2]
  * 1 → \[3, 4]
  * 2 → \[]
  * 3 → \[]
  * 4 → \[]

DFS:

* Start at 0:

  * time\[0] = 1
  * max(dfs(1), dfs(2))
* dfs(1) = 1 + max(dfs(3), dfs(4)) = 1 + 0 = 1
* dfs(2) = 0
* total = 1 + max(1,0) = 2

Answer: **2**

---
