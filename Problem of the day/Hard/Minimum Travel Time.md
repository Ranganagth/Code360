## Problem Summary

You are given an undirected weighted graph with `N` cities and `M` bidirectional roads. The task is to determine the minimum time required to travel all roads at least once and visit all cities, possibly repeating roads if necessary. Return `-1` if it's not possible to visit all cities and roads.

---

# Intuition

This is a variation of the **Chinese Postman Problem (CPP)**, also known as the **Route Inspection Problem**:
* In a graph where all vertices have even degrees, an **Eulerian Circuit** exists - you can traverse all edges exactly once and return to the starting point.
* If there are odd-degree vertices, you must "pair" them and add minimal paths between them to make degrees even - simulating adding extra traversals (not edges) to ensure the final path is valid.

---

# Approach

1. **Edge Case – No Roads:**
   * If `N == 1` and no roads: return 0.
   * If `N > 1` and no roads: return -1.

2. **Build the Graph:**
   * Construct the adjacency list.
   * Track the degree of each vertex.
   * Sum all road weights (`totalWeight`).

3. **DFS to Check Connectivity:**
   * Start DFS from any node with at least one edge.
   * If any node isn't visited, the graph is disconnected → return -1.

4. **Find Odd-Degree Vertices:**
   * If no odd-degree vertices → return `totalWeight`.

5. **All-Pairs Shortest Path (Floyd-Warshall):**
   * Precompute shortest paths between all nodes.

6. **Minimum Weight Perfect Matching (Bitmask DP):**
   * Find the minimum cost to pair up all odd-degree vertices.
   * Add this minimal cost to the original `totalWeight`.

---

# Complexity

1. **Graph Construction:** O(M)
2. **DFS Traversal:** O(N + M)
3. **Floyd-Warshall:** O(N³)
4. **Bitmask DP for Matching:**

   * Number of odd-degree vertices ≤ N
   * DP on $2^k$ masks → $O(k² · 2^k)$, where k = number of odd vertices

**Total Complexity:**
$O(N³ + k²·2^k)$ — efficient enough for moderate `N` (up to \~200)

---

## Example Walkthrough

### Input:

```
N = 4  
roads = [[1,2,3], [2,3,4], [3,4,5], [4,1,6]]
```

### Graph:

* Nodes: 1, 2, 3, 4
* Edges: 1-2(3), 2-3(4), 3-4(5), 4-1(6)
* All nodes have degree 2 → Even

### Result:

* Eulerian Circuit exists.
* All roads visited exactly once.
* Total cost = 3 + 4 + 5 + 6 = **18**

---

### Now Consider:

```
N = 4  
roads = [[1,2,3], [2,3,4], [3,4,5]]
```

* Node 1 and 4 have degree 1 → odd
* Pair (1, 4) via shortest path → 3 + 4 + 5 = 12 (but better is 3 + 4 = 7 if path exists)
* Add shortest path(1-4) = X
* Result = totalWeight + shortestPath(1-4)

---

# Code

```java
import java.util.*;

class Pair {
    int node, weight;
    Pair(int node, int weight) {
        this.node = node;
        this.weight = weight;
    }
}

public class Solution {

    private static void dfs(int node, Map<Integer, List<Pair>> adj, boolean[] visited) {
        visited[node] = true;
        for (Pair neighbor : adj.get(node)) {
            if (!visited[neighbor.node]) {
                dfs(neighbor.node, adj, visited);
            }
        }
    }

    public static int minTravelTime(int n, int[][] roads) {
        if (roads.length == 0) return n == 1 ? 0 : -1;

        Map<Integer, List<Pair>> adj = new HashMap<>();
        int[] degree = new int[n + 1];
        long totalWeight = 0;

        for (int i = 1; i <= n; i++) adj.put(i, new ArrayList<>());
        for (int[] road : roads) {
            int u = road[0], v = road[1], w = road[2];
            adj.get(u).add(new Pair(v, w));
            adj.get(v).add(new Pair(u, w));
            degree[u]++;
            degree[v]++;
            totalWeight += w;
        }

        int startNode = -1;
        for (int i = 1; i <= n; i++) {
            if (degree[i] > 0) {
                startNode = i;
                break;
            }
        }

        boolean[] visited = new boolean[n + 1];
        dfs(startNode, adj, visited);
        for (int i = 1; i <= n; i++) {
            if (!visited[i]) return -1;
        }

        List<Integer> oddVertices = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            if (degree[i] % 2 != 0) oddVertices.add(i);
        }

        if (oddVertices.isEmpty()) return (int) totalWeight;

        long[][] shortestPath = new long[n + 1][n + 1];
        long INF = 1_000_000_000_000L;

        for (int i = 1; i <= n; i++) Arrays.fill(shortestPath[i], INF);
        for (int i = 1; i <= n; i++) shortestPath[i][i] = 0;

        for (int[] road : roads) {
            int u = road[0], v = road[1], w = road[2];
            shortestPath[u][v] = Math.min(shortestPath[u][v], w);
            shortestPath[v][u] = Math.min(shortestPath[v][u], w);
        }

        for (int k = 1; k <= n; k++)
            for (int i = 1; i <= n; i++)
                for (int j = 1; j <= n; j++)
                    if (shortestPath[i][k] < INF && shortestPath[k][j] < INF)
                        shortestPath[i][j] = Math.min(shortestPath[i][j], shortestPath[i][k] + shortestPath[k][j]);

        int m = oddVertices.size();
        long[] dp = new long[1 << m];
        Arrays.fill(dp, INF);
        dp[0] = 0;

        for (int mask = 0; mask < (1 << m); mask++) {
            if (dp[mask] == INF) continue;

            int i = 0;
            while (i < m && ((mask >> i) & 1) != 0) i++;
            if (i == m) continue;

            for (int j = i + 1; j < m; j++) {
                if (((mask >> j) & 1) == 0) {
                    int newMask = mask | (1 << i) | (1 << j);
                    int u = oddVertices.get(i);
                    int v = oddVertices.get(j);
                    if (shortestPath[u][v] == INF) return -1;
                    dp[newMask] = Math.min(dp[newMask], dp[mask] + shortestPath[u][v]);
                }
            }
        }

        return (int) (totalWeight + dp[(1 << m) - 1]);
    }
}
```

---

---

## Code Understanding

```java
import java.util.*;
import java.io.*;

class Pair {
    int node;
    int weight;
    Pair(int node, int weight) {
        this.node = node;
        this.weight = weight;
    }
}

public class Solution {

    private static void dfs(int node, Map<Integer, List<Pair>> adj, boolean[] visited) {
        visited[node] = true;
        for (Pair neighbor : adj.get(node)) {
            if (!visited[neighbor.node]) {
                dfs(neighbor.node, adj, visited);
            }
        }
    }

    public static int minTravelTime(int n, int[][] roads) {
        // Case 1: No roads.
        // If there are no roads, he visits 0 roads.
        // If N=1, he can "visit" the city (start and end there), time 0.
        // If N > 1, and M=0, he cannot visit all N cities. Returns -1.
        // The problem statement: "visit all the 'M' roads and 'N' cities".
        // My previous thought for `roads.length == 0` returning `0` assumed it only refers to roads.
        // But "and N cities" is critical.
        if (roads.length == 0) {
            // If N=1 and M=0, he starts at 1, ends at 1. No roads to travel. Time = 0.
            if (n == 1) {
                return 0;
            } else {
                // If N > 1 and M=0, he cannot visit all cities as they are disconnected.
                return -1;
            }
        }

        Map<Integer, List<Pair>> adj = new HashMap<>();
        int[] degree = new int[n + 1];
        long totalWeight = 0;

        for (int i = 1; i <= n; i++) {
            adj.put(i, new ArrayList<>());
        }

        for (int[] road : roads) {
            int u = road[0];
            int v = road[1];
            int w = road[2];
            adj.get(u).add(new Pair(v, w));
            adj.get(v).add(new Pair(u, w));
            degree[u]++;
            degree[v]++;
            totalWeight += w;
        }

        // Find any node that has a road, to start DFS
        int startNodeForDFS = -1;
        for (int i = 1; i <= n; i++) {
            if (degree[i] > 0) { // Only consider nodes that are part of the graph defined by roads
                startNodeForDFS = i;
                break;
            }
        }
        
        // This 'if' should ideally not be hit if roads.length > 0 and the graph processing is correct
        // but as a safeguard. If all degrees are 0 here, it means roads.length was 0 which is handled above.
        // If startNodeForDFS is still -1 here, it implies an empty graph, which means roads.length was 0.
        // So no need for an explicit check here.

        boolean[] visited = new boolean[n + 1];
        dfs(startNodeForDFS, adj, visited); // Start DFS from a connected node

        // Check if ALL 'N' cities are visited.
        // If N=1, it will pass as visited[1] will be true (assuming startNodeForDFS was 1).
        for (int i = 1; i <= n; i++) {
            if (!visited[i]) {
                // If any city is not visited, it's impossible to visit "all N cities"
                // including isolated ones, or ones in a disconnected component.
                return -1;
            }
        }

        // --- Identify Odd Degree Vertices ---
        List<Integer> oddVertices = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            if (degree[i] % 2 != 0) {
                oddVertices.add(i);
            }
        }

        // --- Chinese Postman Logic ---
        if (oddVertices.isEmpty()) {
            // All vertices have even degrees, an Eulerian Circuit exists.
            // All roads can be traversed exactly once.
            return (int) totalWeight; 
        } else {
            // Some vertices have odd degrees. We need to add edges to make all degrees even.
            
            // Calculate All-Pairs Shortest Paths using Floyd-Warshall
            long[][] shortestPath = new long[n + 1][n + 1];
            long INF_APSP = 1_000_000_000_000L; // A large enough value for infinity

            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    if (i == j) {
                        shortestPath[i][j] = 0;
                    } else {
                        shortestPath[i][j] = INF_APSP;
                    }
                }
            }

            for (int[] road : roads) {
                int u = road[0];
                int v = road[1];
                int w = road[2];
                shortestPath[u][v] = Math.min(shortestPath[u][v], (long)w);
                shortestPath[v][u] = Math.min(shortestPath[v][u], (long)w);
            }

            for (int k = 1; k <= n; k++) {
                for (int i = 1; i <= n; i++) {
                    for (int j = 1; j <= n; j++) {
                        if (shortestPath[i][k] != INF_APSP && shortestPath[k][j] != INF_APSP) {
                            shortestPath[i][j] = Math.min(shortestPath[i][j], shortestPath[i][k] + shortestPath[k][j]);
                        }
                    }
                }
            }

            // --- Minimum Weight Perfect Matching using Bitmask DP ---
            int numOdd = oddVertices.size();
            long[] dp = new long[1 << numOdd];
            Arrays.fill(dp, INF_APSP);
            dp[0] = 0;

            for (int mask = 0; mask < (1 << numOdd); mask++) {
                if (dp[mask] == INF_APSP) {
                    continue;
                }

                int i = -1;
                for (int bit = 0; bit < numOdd; bit++) {
                    if (((mask >> bit) & 1) == 0) {
                        i = bit;
                        break;
                    }
                }

                if (i == -1) {
                    continue;
                }

                int u1 = oddVertices.get(i);

                for (int j = i + 1; j < numOdd; j++) {
                    if (((mask >> j) & 1) == 0) {
                        int u2 = oddVertices.get(j);

                        // If path doesn't exist between odd vertices, it means graph is truly disconnected
                        // even for relevant nodes, return -1.
                        if (shortestPath[u1][u2] == INF_APSP) {
                            return -1; // This scenario implies impossible traversal
                        }

                        int newMask = mask | (1 << i) | (1 << j);
                        dp[newMask] = Math.min(dp[newMask], dp[mask] + shortestPath[u1][u2]);
                    }
                }
            }

            long minAddedCost = dp[(1 << numOdd) - 1];

            if (minAddedCost == INF_APSP) {
                // This means no valid matching could be formed, likely due to disconnected odd vertices
                return -1;
            }

            return (int) (totalWeight + minAddedCost);
        }
    }
}

```

---

