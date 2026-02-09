# Intuition

For every node, the valid ancestors are only those on the path from the root to that node.
While traversing the tree (DFS), maintain information about the most recent ancestor having each possible value.
For the current node, check among all stored ancestor values which ones are coprime with the node’s value and pick the ancestor having the **maximum depth** (closest ancestor).

---

# Approach

1. Build adjacency list of the tree.
2. Maintain an array `lastSeen[val] = (nodeIndex, depth)` storing the closest ancestor having value `val` on the current DFS path.
3. Start DFS from root (node 0).
4. For each node:

   * For all possible values present in `lastSeen`, check `gcd(nodeValue, ancestorValue) == 1`.
   * Among those, select the ancestor with maximum depth → answer for this node.
5. Before visiting children:

   * Save current `lastSeen[nodeValue]`.
   * Update `lastSeen[nodeValue]` with current node.
6. After returning from DFS, restore previous value (backtracking).

---

# Complexity

Let `V` be node value range (bounded small in practice; often ≤50 or ≤1000).

* DFS traversal: `O(N)`
* Coprime checking per node: `O(V)`
* **Total:** `O(N * V)`
* **Space:** `O(N + V)`

---

# Code

```java
import java.util.*;

public class Solution {

    static int MAXV = 51; // assuming node values small (1..50)
    static List<Integer>[] graph;
    static int[] values;
    static int[] result;

    static class Pair {
        int node, depth;
        Pair(int n, int d) { node = n; depth = d; }
    }

    static Pair[] lastSeen = new Pair[MAXV];

    public static ArrayList<Integer> closestCoprimeAncestor(ArrayList<Integer> nodes,
                                                            int[][] edges, int n) {

        values = new int[n];
        for (int i = 0; i < n; i++) values[i] = nodes.get(i);

        graph = new ArrayList[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();

        for (int[] e : edges) {
            graph[e[0]].add(e[1]);
            graph[e[1]].add(e[0]);
        }

        result = new int[n];
        Arrays.fill(result, -1);

        dfs(0, -1, 0);

        ArrayList<Integer> ans = new ArrayList<>();
        for (int x : result) ans.add(x);
        return ans;
    }

    static void dfs(int node, int parent, int depth) {

        int bestNode = -1;
        int bestDepth = -1;

        for (int v = 1; v < MAXV; v++) {
            if (lastSeen[v] != null && gcd(v, values[node]) == 1) {
                if (lastSeen[v].depth > bestDepth) {
                    bestDepth = lastSeen[v].depth;
                    bestNode = lastSeen[v].node;
                }
            }
        }

        result[node] = bestNode;

        Pair prev = lastSeen[values[node]];
        lastSeen[values[node]] = new Pair(node, depth);

        for (int nei : graph[node]) {
            if (nei != parent) {
                dfs(nei, node, depth + 1);
            }
        }

        lastSeen[values[node]] = prev; // backtrack
    }

    static int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
};

```

---

# Example Walkthrough

**Nodes:** `[8,7,4,5]`
**Edges:** `0-1, 0-2, 2-3`

DFS:

* Node 0 (value 8): no ancestors → `-1`
* Node 1 (value 7):

  * Ancestor 0: gcd(7,8)=1 → result[1]=0
* Node 2 (value 4):

  * Ancestor 0: gcd(4,8)=4 → none → `-1`
* Node 3 (value 5):

  * Ancestors: 2 (value 4), 0 (value 8)
  * gcd(5,4)=1, gcd(5,8)=1
  * closest = node 2 → result[3]=2

**Output:**

```
[-1, 0, -1, 2]
```
