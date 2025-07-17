# Intuition

To make the partition as fair as possible, we need to split the tree by removing a single edge such that the difference in total assets of the two resulting subtrees is minimized.

If we know the total sum of all assets, and we compute the sum of assets in one of the resulting subtrees (after removing an edge), we can calculate the other one easily. Thus, we aim to compute subtree sums and evaluate the difference for each edge.

---

# Approach

1. **Build the Tree**: Use an adjacency list to store the tree.
2. **DFS Traversal**: Perform a DFS starting from the root node (`0`) to compute the total asset in each subtree rooted at each node.
3. **Track Differences**:
   * For each subtree rooted at a child node `v` connected to parent `u`, imagine removing the edge between them.
   * One resulting subtree will have total assets equal to `subtreeSum[v]`, the other will be `totalSum - subtreeSum[v]`.
   * Calculate the absolute difference between these two sums.
4. **Find the Minimum Difference** across all such possible edge deletions.

---

# Complexity Analysis

* **Time Complexity**: `O(N)` per test case (for building tree + DFS traversal)
* **Space Complexity**: `O(N)` for adjacency list and subtree sums

---

# Code

```java
import java.util.*;

public class Solution {

    static int totalSum;
    static int minDiff;

    public static int perfectPartition(int n, int[] assets, int[][] edges) {
        List<List<Integer>> tree = new ArrayList<>();
        for (int i = 0; i < n; i++) tree.add(new ArrayList<>());

        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            tree.get(u).add(v);
            tree.get(v).add(u);
        }

        totalSum = 0;
        for (int val : assets) totalSum += val;

        minDiff = Integer.MAX_VALUE;

        boolean[] visited = new boolean[n];
        dfs(0, tree, visited, assets);

        return minDiff;
    }

    private static int dfs(int node, List<List<Integer>> tree, boolean[] visited, int[] assets) {
        visited[node] = true;
        int currSubtreeSum = assets[node];

        for (int neighbor : tree.get(node)) {
            if (!visited[neighbor]) {
                int childSum = dfs(neighbor, tree, visited, assets);
                int otherSum = totalSum - childSum;
                minDiff = Math.min(minDiff, Math.abs(otherSum - childSum));
                currSubtreeSum += childSum;
            }
        }

        return currSubtreeSum;
    }
}
```

---

### **Example Walkthrough**

**Input**:

```
Assets = [10, 20, 30, 40, 50]
Edges: [0-1, 2-0, 1-3, 4-2]
```

1. Total sum = 10 + 20 + 30 + 40 + 50 = **150**
2. Start DFS from node 0:
   * Subtree rooted at node 2 has sum 80 → diff = |150 - 80 - 80| = 10
   * Subtree rooted at node 1 has sum 60 → diff = 90
   * Subtree rooted at node 4 has sum 50 → diff = 50
   * Subtree rooted at node 3 has sum 40 → diff = 70
3. **Minimum difference = 10**

---

### **Sample Output**

```
10
```

