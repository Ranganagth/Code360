# Intuition:

* We are given:
  * `n` boys (1 to n)
  * `m` girls (1 to m)
  * `k` potential edges (boy-girl pair possibilities)

* Goal:
  * Maximize the number of **unique pairs** such that:
    * Each boy and each girl is in **at most one pair**
    * Only allowed pairs from the input are considered

---

# Approach:

1. **Graph Representation**:
   * Build an adjacency list for boys → list of girls they can pair with.

2. **Matching with DFS**:
   * For each boy, try to find an unmatched girl or rearrange matches recursively.
   * Use a `match` array for girls to track who they're matched with.

3. **Result Format**:
   * Return the list of pairs (boy index, girl index).

---
# Complexity Analysis:

* **Worst-case:** `O(N * M)` due to DFS per boy.
* **Efficient enough** for `N, M ≤ 100`

---
# Code:

```java
import java.util.*;

public class Solution {
    public static ArrayList<ArrayList<Integer>> danceTogether(int[][] edges, int n, int m, int k) {
        // Adjacency list for boys
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i <= n; i++) graph.add(new ArrayList<>());

        for (int[] edge : edges) {
            int boy = edge[0];
            int girl = edge[1];
            graph.get(boy).add(girl);
        }

        // match[girl] = boy
        int[] match = new int[m + 1];
        Arrays.fill(match, -1);

        ArrayList<ArrayList<Integer>> result = new ArrayList<>();

        for (int boy = 1; boy <= n; boy++) {
            boolean[] visited = new boolean[m + 1];
            if (dfs(boy, graph, visited, match)) {
                // successful matching done in dfs
            }
        }

        // Collect pairs from match[]
        for (int girl = 1; girl <= m; girl++) {
            if (match[girl] != -1) {
                ArrayList<Integer> pair = new ArrayList<>();
                pair.add(match[girl]);
                pair.add(girl);
                result.add(pair);
            }
        }

        return result;
    }

    private static boolean dfs(int boy, List<List<Integer>> graph, boolean[] visited, int[] match) {
        for (int girl : graph.get(boy)) {
            if (visited[girl]) continue;
            visited[girl] = true;

            if (match[girl] == -1 || dfs(match[girl], graph, visited, match)) {
                match[girl] = boy;
                return true;
            }
        }
        return false;
    }
}
```

---

### Example:

For input:

```
n = 3, m = 2, edges = [[1 1], [1 2], [2 1], [3 1]]
```

Possible pairs:

* (1,2), (2,1)
* or (1,2), (3,1)

Max number of pairs = 2

---
