# Intuition

* Applicants and internships form two disjoint sets in a **bipartite graph**.
* An edge between applicant `i` and internship `j` exists if `G[i][j] == 1`.
* We aim to **maximize the number of matched pairs** where:
  * Each applicant is matched to **at most one** internship.
  * Each internship is matched to **at most one** applicant.

---

# Approach

We use **DFS-based augmentation** to find the maximum bipartite matching:

1. Maintain an array `match` of size `m` where `match[j] = i` means internship `j` is assigned to applicant `i`.
2. For each applicant `i`, attempt to assign them to an internship `j`:
   * If `G[i][j] == 1` (applicant is interested in internship),
   * And if internship `j` is **not taken** or **can be reassigned**, assign it.
3. Use a DFS to attempt all possible assignments recursively.

---

# Complexity

* Worst-case time: **O(N × M)** per test case, since for each applicant, we may explore up to `M` internships.

---

# Code

```java
import java.util.*;

class Solution {

  public static int internships(int n, int m, int[][] g) {
    int[] match = new int[m];
    Arrays.fill(match, -1); // -1 means internship not assigned to anyone yet

    int result = 0;
    for (int i = 0; i < n; i++) {
      boolean[] visited = new boolean[m];
      if (canAssign(i, g, visited, match)) {
        result++;
      }
    }

    return result;
  }

  private static boolean canAssign(int applicant, int[][] g, boolean[] visited, int[] match) {
    int m = g[0].length;
    for (int internship = 0; internship < m; internship++) {
      if (g[applicant][internship] == 1 && !visited[internship]) {
        visited[internship] = true;

        // If internship is unassigned OR previous assignee can be reassigned
        if (match[internship] == -1 || canAssign(match[internship], g, visited, match)) {
          match[internship] = applicant;
          return true;
        }
      }
    }
    return false;
  }
}
```

---

### **Example Walkthrough:**

Test Case:

```
3 5
1 1 0 1 1
0 1 0 0 1
1 1 0 1 1
```

* Applicant 0 → 0,1,3,4
* Applicant 1 → 1,4
* Applicant 2 → 0,1,3,4

One possible matching:
* Applicant 0 → 0
* Applicant 1 → 1
* Applicant 2 → 4

Maximum matches = **3**

---
