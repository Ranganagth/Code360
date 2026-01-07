# Intuition

“Beautiful” = articulation point. A vertex whose removal increases the number of connected components. In DFS, this happens only if the vertex is a **bridge-keeper**: there exists a child subtree that cannot reach any ancestor except through this vertex.

---

# Approach

Use DFS + timestamps (Tarjan’s idea).

**Maintain:**

* `disc[u]`: discovery time of `u`
* `low[u]`: earliest reachable discovery time from `u`
* `parent[u]`
* `isArt[u]`: mark articulation vertices

**DFS rules:**

1. `disc[u] = low[u] = ++time`
2. For each neighbor `v`:

   * If `v` unvisited:
     set parent, DFS on `v`, then update `low[u] = min(low[u], low[v])`

     * Root case: root is articulation if it has **>1 children**
     * Non-root: if `low[v] >= disc[u]`, `u` is articulation
   * Else if `v != parent[u]`: back-edge → `low[u] = min(low[u], disc[v])`

Count vertices marked articulation.

---

# Complexity

- **Time:** `O(N + M)`
- **Space:** `O(N + M)` (adjacency + recursion + arrays)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    static int time;

    public static int beautifulVertices(int n, int m, int[][] edges) {

        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) adj.add(new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1];
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        int[] disc = new int[n + 1];
        int[] low  = new int[n + 1];
        int[] parent = new int[n + 1];
        boolean[] isArt = new boolean[n + 1];
        Arrays.fill(parent, -1);
        time = 0;

        for (int u = 1; u <= n; u++) {
            if (disc[u] == 0)
                dfs(u, adj, disc, low, parent, isArt);
        }

        int count = 0;
        for (int u = 1; u <= n; u++)
            if (isArt[u]) count++;

        return count;
    }

    private static void dfs(int u,
                            List<List<Integer>> adj,
                            int[] disc,
                            int[] low,
                            int[] parent,
                            boolean[] isArt) {

        disc[u] = low[u] = ++time;
        int children = 0;

        for (int v : adj.get(u)) {

            if (disc[v] == 0) {      // tree edge
                parent[v] = u;
                children++;

                dfs(v, adj, disc, low, parent, isArt);

                low[u] = Math.min(low[u], low[v]);

                if (parent[u] == -1 && children > 1)
                    isArt[u] = true;

                if (parent[u] != -1 && low[v] >= disc[u])
                    isArt[u] = true;

            } else if (v != parent[u]) {   // back edge
                low[u] = Math.min(low[u], disc[v]);
            }
        }
    }
};

```

---

# Example walkthrough 

N=3, edges form a triangle

DFS finds each `low[v] < disc[u]` due to back edges, so no `low[v] >= disc[u]` for any non-root. Root has only one child. No articulation points → answer 0.
