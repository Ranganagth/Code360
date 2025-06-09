# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution{
    static int maximumTreeHeight(int n, int [][]edges){
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i <= n; i++) {
            adj.add(new ArrayList<>());
        }

        for (int[] edge: edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }

        int[] result1 = bfs(1, n, adj);
        int u = result1[0];

        int[] result2 = bfs(u, n, adj);
        int diameter = result2[1];

        return diameter;
    }

    private static int[] bfs(int startNode, int n, ArrayList<ArrayList<Integer>> adj) {
        Queue<Integer> q = new LinkedList<>();
        int[] dist = new int[n + 1];
        Arrays.fill(dist, -1);

        q.offer(startNode);
        dist[startNode] = 0;

        int farthestNode = startNode;
        int maxDist = 0;

        while (!q.isEmpty()) {
            int u = q.poll();

            if (dist[u] > maxDist) {
                maxDist = dist[u];
                farthestNode = u;
            }

            for (int v : adj.get(u)) {
                if (dist[v] == -1) {
                    dist[v] = dist[u] + 1;
                    q.offer(v);
                }
            }
        }

        return new int[]{farthestNode, maxDist};
    }
}

```