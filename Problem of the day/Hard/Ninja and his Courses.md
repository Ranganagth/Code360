# Intuition

`N ≤ 15`, so every subset of courses can be represented as a **bitmask**.
A bitmask `mask` represents courses already completed.

For each course, maintain a **prerequisite mask**.

At any state:

* determine which courses are available (all prerequisites satisfied)
* choose **up to K courses** from these available courses
* move to the next mask.

The problem becomes **BFS on bitmask states** where each BFS level represents one semester.

---

# Approach

1. Build `pre[i]` mask for each course.
2. Use `mask` to represent completed courses.
3. Start BFS from `mask = 0`.
4. For each mask:

   * compute available courses whose prerequisites are satisfied.
   * remove already taken courses.
5. If available courses ≤ `K` → take all.
6. Otherwise enumerate all **subsets of size K**.
7. Push new mask into BFS queue.
8. Stop when mask == `(1<<N)-1`.

---

# Complexity

* **Time complexity:** $(O(3^N))$ worst case (bounded because $(N ≤ 15)$)
* **Space complexity:** $(O(2^N))$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int minNumOfSemesters(int n, int[][] dependencies, int k) {

        int[] pre = new int[n];

        for (int[] d : dependencies) {
            int u = d[0] - 1;
            int v = d[1] - 1;
            pre[v] |= (1 << u);
        }

        int target = (1 << n) - 1;

        Queue<Integer> q = new LinkedList<>();
        boolean[] visited = new boolean[1 << n];

        q.offer(0);
        visited[0] = true;

        int semester = 0;

        while (!q.isEmpty()) {

            int size = q.size();

            while (size-- > 0) {

                int mask = q.poll();

                if (mask == target)
                    return semester;

                int available = 0;

                for (int i = 0; i < n; i++) {
                    if ((mask & (1 << i)) == 0 &&
                        (pre[i] & mask) == pre[i]) {

                        available |= (1 << i);
                    }
                }

                available &= ~mask;

                List<Integer> subsets = new ArrayList<>();

                for (int sub = available; sub > 0; sub = (sub - 1) & available) {

                    if (Integer.bitCount(sub) <= k)
                        subsets.add(sub);
                }

                if (subsets.isEmpty())
                    subsets.add(available);

                for (int sub : subsets) {

                    int next = mask | sub;

                    if (!visited[next]) {
                        visited[next] = true;
                        q.offer(next);
                    }
                }
            }

            semester++;
        }

        return -1;
    }
};

```

---

# Example walkthrough

Example:

```
N = 4
Dependencies = [2→1, 3→1, 1→4]
K = 2
```

Prerequisite masks:

```
1: {2,3}
4: {1}
```

Semester progression:

```
S1 → take {2,3}
S2 → take {1}
S3 → take {4}
```

Total semesters:

```
3
```
