# Intuition

This is a **shortest path problem on states**.

Each board configuration is a node.
Each valid swap of `0` creates a new state (edge).

Goal:
Reach target state `"123450"` with **minimum moves** → BFS.

State space is small (6! = 720), so BFS is feasible.

---

# Approach

1. Convert board → string (e.g., `"120453"`)
2. Define neighbors for each index (precomputed adjacency)
3. BFS:

   * Start from initial state
   * Swap `0` with valid neighbors
   * Track visited states
4. Stop when target `"123450"` is reached
5. If BFS ends without reaching target → return -1

Index mapping (2x3 → 1D):

```
0 1 2
3 4 5
```

Neighbors:

```
0 → [1,3]
1 → [0,2,4]
2 → [1,5]
3 → [0,4]
4 → [1,3,5]
5 → [2,4]
```

---

# Complexity

* **Time complexity:**  $$O(6!) \approx O(720)$$

* **Space complexity:**  $$O(720)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {
	public static int slidingPuzzle(List<List<Integer>> board) {

		String target = "123450";
		StringBuilder sb = new StringBuilder();

		for (List<Integer> row : board) {
			for (int num : row) {
				sb.append(num);
			}
		}

		String start = sb.toString();

		int[][] neighbors = {
			{1,3}, {0,2,4}, {1,5},
			{0,4}, {1,3,5}, {2,4}
		};

		Queue<String> q = new LinkedList<>();
		Set<String> visited = new HashSet<>();

		q.offer(start);
		visited.add(start);

		int moves = 0;

		while (!q.isEmpty()) {

			int size = q.size();

			for (int i = 0; i < size; i++) {

				String curr = q.poll();

				if (curr.equals(target)) return moves;

				int zeroIndex = curr.indexOf('0');

				for (int next : neighbors[zeroIndex]) {

					char[] arr = curr.toCharArray();

					char temp = arr[zeroIndex];
					arr[zeroIndex] = arr[next];
					arr[next] = temp;

					String nextState = new String(arr);

					if (!visited.contains(nextState)) {
						visited.add(nextState);
						q.offer(nextState);
					}
				}
			}

			moves++;
		}

		return -1;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Input:
[1 2 0
4 5 3]

String: `"120453"`

Step 1:
Swap 0 with index 5 → `"123450"`

Reached target in 1 move

## Example 2:

Input:
[1 2 3
5 4 0]

String: `"123540"`

This configuration is unsolvable (parity mismatch)

BFS explores all states but never reaches `"123450"`

Answer = -1
