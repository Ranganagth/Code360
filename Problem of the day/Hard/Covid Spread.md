# Intuition

Infection spreads level by level (day by day) from already infected houses.

This is a **multi-source BFS** problem:

* All initially infected cells (value = 2) act as starting points
* Each BFS level = 1 day
* Spread to adjacent (4 directions)

Goal:

* Count minimum layers needed to infect all 1’s
* If any 1 remains unreachable → return -1

---

# Approach

1. Traverse grid:

   * Push all infected cells (2) into queue
   * Count total non-infected (1)
2. Perform BFS:

   * For each level:

     * Spread infection to adjacent cells
     * Convert 1 → 2
     * Decrease fresh count
3. Track days (levels of BFS)
4. If fresh count becomes 0 → return days
5. Else → return -1

---

# Complexity

* **Time complexity:**
  $$O(N \cdot M)$$

* **Space complexity:**
  $$O(N \cdot M)$$

---

# Code

```Java
import java.util.*;

public class Solution 
{
	public static int covidSpread(ArrayList<ArrayList<Integer>> houses) 
	{
		int n = houses.size();
		int m = houses.get(0).size();

		Queue<int[]> q = new LinkedList<>();
		int fresh = 0;

		// Initialize
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (houses.get(i).get(j) == 2) {
					q.offer(new int[]{i, j});
				} else if (houses.get(i).get(j) == 1) {
					fresh++;
				}
			}
		}

		if (fresh == 0) return 0;

		int days = 0;
		int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

		while (!q.isEmpty()) {

			int size = q.size();
			boolean spread = false;

			for (int i = 0; i < size; i++) {

				int[] curr = q.poll();
				int x = curr[0], y = curr[1];

				for (int[] d : dirs) {
					int nx = x + d[0];
					int ny = y + d[1];

					if (nx >= 0 && ny >= 0 && nx < n && ny < m &&
						houses.get(nx).get(ny) == 1) {

						houses.get(nx).set(ny, 2);
						q.offer(new int[]{nx, ny});
						fresh--;
						spread = true;
					}
				}
			}

			if (spread) days++;
		}

		return fresh == 0 ? days : -1;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Grid:
2 2
0 1

Initial:

* Infected: (0,0), (0,1)
* Fresh: 1

Day 1:

* (0,1) infects (1,1)

All infected → days = 1

## Example 2:

Grid:
1 0 1
2 1 0

Initial:

* Infected: (1,0)
* Fresh: 3

Propagation blocked by 0’s → some cells unreachable

Result = -1
