# Solution

```java
import java.util.ArrayList;
import java.util.*;

public class Solution {
	public static int stronglyConnectedComponents(int v, ArrayList<ArrayList<Integer>> edges) {
		List<List<Integer>> adj = new ArrayList<>();
		for(int i = 0; i < v; i++) {
			adj.add(new ArrayList<>());
		}

		for (ArrayList<Integer> edge : edges) {
			adj.get(edge.get(0)).add(edge.get(1));
		}

		boolean[] visited = new boolean[v];
		Stack<Integer> stack = new Stack<>();

		for (int i = 0; i < v; i++) {
			if (!visited[i]) {
				dfs(i, adj, visited, stack);
			}
		}

		List<List<Integer>> transpose = new ArrayList<>();
		for (int i = 0; i < v; i++) {
			transpose.add(new ArrayList<>());
		}

		for (int u = 0; u < v; u++) {
			for (int nei : adj.get(u)) {
				transpose.get(nei).add(u);
			}
		}

		Arrays.fill(visited, false);
		int sccCount = 0;

		while (!stack.isEmpty()) {
			int node = stack.pop();
			if (!visited[node]) {
				dfsTranspose(node, transpose, visited);
				sccCount++;
			}
		}

		return sccCount;
	}

	private static void dfs(int node, List<List<Integer>> adj, boolean[] visited, Stack<Integer> stack) {
		visited[node] = true;
		for (int nei : adj.get(node)) {
			if (!visited[nei]) {
				dfs(nei, adj, visited, stack);
			}
		}
		stack.push(node);
	}

	private static void dfsTranspose(int node, List<List<Integer>> adj, boolean[] visited) {
		visited[node] = true;
		for (int nei : adj.get(node)) {
			if (!visited[nei]) {
			dfsTranspose(nei, adj, visited);
			}
		}
	}
}

```