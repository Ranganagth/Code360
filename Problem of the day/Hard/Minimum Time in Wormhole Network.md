# Intuition

We need the **minimum time** from source `(sx, sy)` to destination `(dx, dy)` considering:

* Moving normally in grid → **Manhattan distance** between two points.
* Wormhole usage → special edge: from entry → exit (or vice versa) in **given cost**.

So the universe becomes a **graph**:

* **Nodes** = source, destination, and each wormhole endpoint.
* **Edges** =

  * Between any two nodes → their Manhattan distance.
  * Between wormhole endpoints → the wormhole travel cost.

Now, the problem reduces to finding the **shortest path from source to destination** in this graph.

---

# Approach

1. Build a list of nodes:

   * Index `0` → source `(sx, sy)`
   * Index `1` → destination `(dx, dy)`
   * From index `2` onward → wormhole endpoints (two nodes per wormhole).

2. Construct adjacency list / matrix:

   * For every pair `(i, j)` compute Manhattan distance.
   * For wormholes, add special edge between its two endpoints with wormhole cost (bi-directional).

3. Run **Dijkstra’s Algorithm** (since edge weights are non-negative) from source to destination.

4. Return the shortest distance.

---

# Complexity

* Nodes = `2 + 2N` (≤ 402).
* Edges ≈ `O(V^2)` since we may connect every node pair.
* Dijkstra with priority queue: **O(V² log V)** in worst case, but with `V ≤ 402` this is totally fine.

---

# Code

```java
import java.util.*;

class Wormhole {
    int startX, startY, endX, endY, cost;
    public Wormhole(int startX, int startY, int endX, int endY, int cost) {
        this.startX = startX;
        this.startY = startY;
        this.endX = endX;
        this.endY = endY;
        this.cost = cost;
    }
}

public class Solution {

    static class Node {
        int x, y;
        Node(int x, int y) { this.x = x; this.y = y; }
    }

    public static int minTimeInWormholeNetwork(int n, int sx, int sy, int dx, int dy, Wormhole[] wormhole) {
        // Step 1: build list of nodes
        List<Node> nodes = new ArrayList<>();
        nodes.add(new Node(sx, sy)); // 0 = source
        nodes.add(new Node(dx, dy)); // 1 = destination
        for (Wormhole w : wormhole) {
            nodes.add(new Node(w.startX, w.startY));
            nodes.add(new Node(w.endX, w.endY));
        }

        int V = nodes.size();
        int[][] graph = new int[V][V];

        // Step 2: initialize graph with Manhattan distances
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (i == j) graph[i][j] = 0;
                else graph[i][j] = manhattan(nodes.get(i), nodes.get(j));
            }
        }

        // Step 3: add wormhole edges
        for (int i = 0; i < n; i++) {
            int u = 2 + 2 * i;     // start endpoint
            int v = 2 + 2 * i + 1; // end endpoint
            int cost = wormhole[i].cost;
            graph[u][v] = graph[v][u] = Math.min(graph[u][v], cost);
        }

        // Step 4: Dijkstra from source (0) to destination (1)
        return dijkstra(graph, V, 0, 1);
    }

    private static int manhattan(Node a, Node b) {
        return Math.abs(a.x - b.x) + Math.abs(a.y - b.y);
    }

    private static int dijkstra(int[][] graph, int V, int src, int dest) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[1]));
        pq.add(new int[]{src, 0});

        boolean[] visited = new boolean[V];

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int u = cur[0];
            if (visited[u]) continue;
            visited[u] = true;

            for (int v = 0; v < V; v++) {
                if (!visited[v]) {
                    int newDist = dist[u] + graph[u][v];
                    if (newDist < dist[v]) {
                        dist[v] = newDist;
                        pq.add(new int[]{v, newDist});
                    }
                }
            }
        }
        return dist[dest];
    }
}
```

---

# Example Walkthrough

Input:

```
0 0 20 20
2
2 2 10 10 5
8 8 16 16 5
```

* Source = `(0,0)`, Destination = `(20,20)`
* Wormhole 1: `(2,2)` ↔ `(10,10)` cost 5
* Wormhole 2: `(8,8)` ↔ `(16,16)` cost 5

Graph edges:

* From `(0,0)` to `(2,2)` = 4
* `(2,2)` to `(10,10)` = 5 (wormhole)
* `(10,10)` to `(8,8)` = 4
* `(8,8)` to `(16,16)` = 5 (wormhole)
* `(16,16)` to `(20,20)` = 8
  Total = 26 → minimal path.

---
