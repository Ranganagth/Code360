# Intuition

Malware spreads only within connected components. If a component has **only one infected node**, removing it can **save the entire component**. So, we want to:

* Find such components with exactly **one infected node**.
* Among those, choose the infected node that saves the **most computers**.
* If there's a tie, return the **smallest index**.

---

# Approach

1. **Use Union-Find** to group computers into connected components.
2. Count the **size** of each component.
3. Track how many **infected nodes** exist per component.
4. For each infected node:

   * If it’s the **only infected node in its component**, compute the **number of nodes saved** by removing it.
5. Among all options, select the infected node that **saves the most computers**.
6. If no such node exists (i.e., all components have multiple infections), return the **infected node with the smallest index**.

---

# Complexity

* **Time Complexity:**
  $O(N^2 + N \cdot \alpha(N))$
  Where $N$ is the number of nodes (computers). $\alpha(N)$ is the inverse Ackermann function for Union-Find operations.

* **Space Complexity:**
  $O(N)$

---

# Code

```java
import java.util.*;

public class Solution {

    public static int minimizeMalwareSpread(int n, int[][] graph, int m, int[] infected) {
        int[] parent = new int[n];

        // Initialize Union-Find
        for (int i = 0; i < n; i++) parent[i] = i;

        // Union connected computers
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (graph[i][j] == 1) {
                    union(i, j, parent);
                }
            }
        }

        // Count size of each component
        int[] componentSize = new int[n];
        for (int i = 0; i < n; i++) {
            int root = find(i, parent);
            componentSize[root]++;
        }

        // Count how many infected nodes are in each component
        int[] infectedCount = new int[n];
        for (int node : infected) {
            int root = find(node, parent);
            infectedCount[root]++;
        }

        Arrays.sort(infected); // To handle tie by smallest index

        int result = infected[0];
        int maxSaved = -1;

        for (int node : infected) {
            int root = find(node, parent);
            if (infectedCount[root] == 1) {
                int saved = componentSize[root];
                if (saved > maxSaved) {
                    maxSaved = saved;
                    result = node;
                } else if (saved == maxSaved && node < result) {
                    result = node;
                }
            }
        }

        return result;
    }

    private static int find(int x, int[] parent) {
        if (parent[x] != x) parent[x] = find(parent[x], parent);
        return parent[x];
    }

    private static void union(int x, int y, int[] parent) {
        int rootX = find(x, parent);
        int rootY = find(y, parent);
        if (rootX != rootY) parent[rootY] = rootX;
    }

    // Optional main method to run multiple test cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        while (T-- > 0) {
            int n = sc.nextInt();
            int m = sc.nextInt();
            int[][] graph = new int[n][n];
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                    graph[i][j] = sc.nextInt();

            int[] infected = new int[m];
            for (int i = 0; i < m; i++)
                infected[i] = sc.nextInt();

            System.out.println(minimizeMalwareSpread(n, graph, m, infected));
        }
    }
}
```

---
