# Intuition

We need to assign each student exactly one cycle, following the rules:

1. Always assign the **shortest Manhattan distance** pair.
2. If there’s a tie → pick the **student with smaller index**.
3. If still tied → pick the **cycle with smaller index**.
4. Repeat until all students are assigned.

# Approach

* Compute Manhattan distances between **every student and every cycle**.
* Store them in a list of `(distance, studentIndex, cycleIndex)`.
* Sort the list by:
  1. Distance
  2. Student index
  3. Cycle index
* Iterate through the sorted list:
  * If both student and cycle are unassigned → assign that pair.
* Continue until all students are assigned.

# Complexity

* Distance computation: `O(N * M)`
* Sorting pairs: `O(N * M * log(N * M))`
* Assignment scan: `O(N * M)`

Given constraints (`N, M ≤ 1000`), this is efficient enough.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> allocateCycles(ArrayList<ArrayList<Integer>> students, ArrayList<ArrayList<Integer>> cycles) {
        int n = students.size();
        int m = cycles.size();

        // List to hold (distance, studentIndex, cycleIndex)
        List<int[]> pairs = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            int sx = students.get(i).get(0);
            int sy = students.get(i).get(1);

            for (int j = 0; j < m; j++) {
                int cx = cycles.get(j).get(0);
                int cy = cycles.get(j).get(1);
                int dist = Math.abs(sx - cx) + Math.abs(sy - cy);
                pairs.add(new int[]{dist, i, j});
            }
        }

        // Sort by distance, then student index, then cycle index
        Collections.sort(pairs, (a, b) -> {
            if (a[0] != b[0]) return a[0] - b[0]; // distance
            if (a[1] != b[1]) return a[1] - b[1]; // student index
            return a[2] - b[2]; // cycle index
        });

        int[] result = new int[n];
        Arrays.fill(result, -1);
        boolean[] cycleAssigned = new boolean[m];
        int assigned = 0;

        for (int[] p : pairs) {
            int dist = p[0], student = p[1], cycle = p[2];
            if (result[student] == -1 && !cycleAssigned[cycle]) {
                result[student] = cycle;
                cycleAssigned[cycle] = true;
                assigned++;
                if (assigned == n) break; // all students assigned
            }
        }

        ArrayList<Integer> ans = new ArrayList<>();
        for (int val : result) ans.add(val);
        return ans;
    }
}
```

---
