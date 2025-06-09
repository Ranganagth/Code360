# Solution

```java
import java.util.ArrayList;

public class Solution {

	private static int N_ROWS;
	private static int M_COLS;
	private static boolean[][] visited;
	private static ArrayList<ArrayList<Integer>> GRID;

	private static void dfs(int r, int c) {
		if (r < 0 || r >= N_ROWS || c < 0 || c >= M_COLS || visited[r][c] || GRID.get(r).get(c) == 0) {
			return;
		}

		visited[r][c] = true;

		dfs(r + 1, c);
		dfs(r - 1, c);
		dfs(r, c + 1);
		dfs(r, c - 1);
	}

	private static int countConnectedComponents(ArrayList<ArrayList<Integer>> grid) {
		GRID = grid;
		N_ROWS = grid.size();
		M_COLS = grid.get(0).size();

		visited = new boolean[N_ROWS][M_COLS];
		int components = 0;

		for (int i = 0; i < N_ROWS; i++) {
			for (int j = 0; j < M_COLS; j++) {
				if (GRID.get(i).get(j) == 1 && !visited[i][j]) {
					dfs(i, j);
					components++;
				}
			}
		}
		return components;
	}

	public static int minCities(ArrayList<ArrayList<Integer>> grid, int n, int m) {
		int initialComponents = countConnectedComponents(grid);

		if (initialComponents != 1) {
			return 0;
		}

		for (int r = 0; r < n; r++) {
			for (int c = 0; c < m; c++) {
				if (grid.get(r).get(c) == 1) {
					grid.get(r).set(c, 0);

					int componentsAfterRemoval = countConnectedComponents(grid);

					if (componentsAfterRemoval != 1) {
						grid.get(r).set(c, 1);
						return 1;
					}

					grid.get(r).set(c, 1);
				}
			}
		}

		return 2;
	}
}

```