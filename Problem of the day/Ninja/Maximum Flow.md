> We can solve it using **Edmonds–Karp Algorithm** (which is an implementation of Ford–Fulkerson using BFS).

---

# Intuition

We need to push as much flow as possible from **source (house 1)** to **sink (house N)** along the pipes, while respecting their capacity.
This is the **classic max-flow problem**:

* Think of each pipe as a directed edge with capacity.
* We repeatedly find paths from **1 → N** that still have available capacity, push flow, and update residual capacities until no more augmenting paths exist.

---

# Approach

1. **Build adjacency list (residual graph):**

   * Store all pipes with capacities.
   * If multiple pipes exist between two houses, sum their capacities.
   * Maintain residual capacity for each edge.

2. **Use Edmonds–Karp (BFS-based Ford–Fulkerson):**

   * While there exists an augmenting path from source (1) to sink (N):

     * Use BFS to find the path and also record the minimum residual capacity along it (bottleneck).
     * Push this flow through the path.
     * Update residual graph (decrease forward edge, increase reverse edge).

3. **Stop when no more augmenting paths exist.**

---

# Complexity

* **Time Complexity:**
  Edmonds–Karp runs in **O(V \* E²)**.
  With `V = N ≤ 500` and `E ≤ N*(N-1)/2 ≈ 125k`, this is feasible within 1s.

* **Space Complexity:**
  O(V²) if adjacency matrix, or O(E) if adjacency list. Here we use adjacency list → **O(E)**.

---

# Code

```java
import java.util.*;

public class Solution {

    static class Edge {
        int to, capacity, rev;
        Edge(int to, int capacity, int rev) {
            this.to = to;
            this.capacity = capacity;
            this.rev = rev;
        }
    }

    public static int findMaxFlow(int n, int m, ArrayList<ArrayList<Integer>> pipes) {
        // Build graph
        List<Edge>[] graph = new ArrayList[n + 1];
        for (int i = 0; i <= n; i++) graph[i] = new ArrayList<>();

        for (ArrayList<Integer> pipe : pipes) {
            int u = pipe.get(0);
            int v = pipe.get(1);
            int cap = pipe.get(2);

            // forward edge
		    graph[u].add(new Edge(v, cap, graph[v].size()));
		    // reverse edge (same capacity since pipe is bidirectional)
		    graph[v].add(new Edge(u, cap, graph[u].size() - 1));
        }

        return maxFlow(graph, 1, n);
    }

    private static int maxFlow(List<Edge>[] graph, int s, int t) {
        int flow = 0, n = graph.length;
        int[] level = new int[n];
        int[] iter = new int[n];

        while (bfs(graph, s, t, level)) {
            Arrays.fill(iter, 0);
            int f;
            while ((f = dfs(graph, s, t, Integer.MAX_VALUE, iter, level)) > 0) {
                flow += f;
            }
        }
        return flow;
    }

    private static boolean bfs(List<Edge>[] graph, int s, int t, int[] level) {
        Arrays.fill(level, -1);
        Queue<Integer> q = new LinkedList<>();
        level[s] = 0;
        q.offer(s);

        while (!q.isEmpty()) {
            int u = q.poll();
            for (Edge e : graph[u]) {
                if (e.capacity > 0 && level[e.to] < 0) {
                    level[e.to] = level[u] + 1;
                    q.offer(e.to);
                }
            }
        }
        return level[t] >= 0;
    }

    private static int dfs(List<Edge>[] graph, int u, int t, int f, int[] iter, int[] level) {
        if (u == t) return f;
        for (; iter[u] < graph[u].size(); iter[u]++) {
            Edge e = graph[u].get(iter[u]);
            if (e.capacity > 0 && level[u] + 1 == level[e.to]) {
                int d = dfs(graph, e.to, t, Math.min(f, e.capacity), iter, level);
                if (d > 0) {
                    e.capacity -= d;
                    graph[e.to].get(e.rev).capacity += d;
                    return d;
                }
            }
        }
        return 0;
    }
}
```

---

# Example Walkthrough

Input:

```
N = 4, M = 3
Pipes = [[1,2,2], [1,3,4], [3,4,3]]
```

* Graph:

  * 1 → 2 (cap 2)
  * 1 → 3 (cap 4)
  * 3 → 4 (cap 3)

Step 1: BFS finds path `1 → 3 → 4`, bottleneck = 3. Push 3 units.
Step 2: No more augmenting path with available capacity to reach node 4.
Total flow = 3.

Output: `3`.

---
