# Intuition

Points form groups based on a **connectivity rule**, not direct pairwise constraint.

If:

* A is close to B
* B is close to C

Then A, B, C belong to the same group even if A and C are far.

This is a **graph connectivity problem**:

* Each point = node
* Edge exists if distance ≤ K
* Answer = number of **connected components**

Efficient structure: **Disjoint Set (Union-Find)**

Avoid square root:
Compare squared distance ≤ K²

---

# Approach

1. Initialize DSU (parent array).
2. For every pair (i, j):

   * Compute squared distance:
$$     (x1 - x2)^2 + (y1 - y2)^2
$$
   * If ≤ K² → union(i, j)
3. Count unique parents → number of groups

Optimizations:

* Path compression
* Union by rank

---

# Complexity

* **Time complexity:**
  $$O(n^2 \cdot \alpha(n))$$

* **Space complexity:**
  $$O(n)$$

---

# Code

```Java id="l9y3o2"
public class Solution {

	static class DSU {
		int[] parent, rank;

		DSU(int n) {
			parent = new int[n];
			rank = new int[n];
			for (int i = 0; i < n; i++) {
				parent[i] = i;
			}
		}

		int find(int x) {
			if (parent[x] != x) {
				parent[x] = find(parent[x]); // path compression
			}
			return parent[x];
		}

		void union(int x, int y) {
			int px = find(x);
			int py = find(y);

			if (px == py) return;

			if (rank[px] < rank[py]) {
				parent[px] = py;
			} else if (rank[px] > rank[py]) {
				parent[py] = px;
			} else {
				parent[py] = px;
				rank[px]++;
			}
		}
	}

	public static int groupPoints(int n, int k, int[][] points) {

		DSU dsu = new DSU(n);
		int kSq = k * k;

		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {

				int dx = points[i][0] - points[j][0];
				int dy = points[i][1] - points[j][1];

				if (dx * dx + dy * dy <= kSq) {
					dsu.union(i, j);
				}
			}
		}

		int count = 0;
		for (int i = 0; i < n; i++) {
			if (dsu.find(i) == i) {
				count++;
			}
		}

		return count;
	}
}
```

---

# Example Walkthrough

## Example 1: Step by step explanation

Points = [(0,0), (1,1), (3,3)], K = 2

Compute distances:

* (0,0) ↔ (1,1) → 2 ≤ 4 → union
* (1,1) ↔ (3,3) → 8 > 4 → no union
* (0,0) ↔ (3,3) → 18 > 4 → no union

Groups:

* {0,1}
* {2}

Answer = 2

## Example 2:

Points = [(0,0), (1,1), (2,2)], K = 1

Distances:

* All distances > 1

No unions formed

Groups:

* {0}, {1}, {2}

Answer = 3
