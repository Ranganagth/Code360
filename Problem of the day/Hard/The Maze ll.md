# Intuition

* Ninja can’t stop mid-way, only when hitting a wall.
* From each position, you can "roll" in 4 directions until hitting a wall, count how many steps were rolled, and then stop there.
* We want the **minimum total distance** to reach the destination.

So, this becomes a **shortest path problem** on a graph where:

* Nodes = valid stopping points.
* Edges = roll in a direction until hitting a wall.
* Edge weight = number of steps rolled.

---

# Approach

1. Use **Dijkstra’s algorithm**:

   * Maintain a `dist` matrix with shortest distance to each cell.
   * Initialize `dist[start] = 0`.
   * Use a min-heap (priority queue) with `(distance, row, col)`.
   * Pop the smallest distance node, and for each direction:

     * Roll until hitting a wall, count steps.
     * If `newDist < dist[newPos]`, update and push into queue.
2. Stop when reaching destination, or return `-1` if not reachable.

---

# Complexity

* At most $N \times M$ nodes.
* Each relaxation involves rolling in a direction: O(N + M) worst case per move.
* Overall: **O(N × M × log(N × M))**, which works fine for $N, M \leq 100$.

---

# Code

```java
import java.util.*;

class Solution {

  static class State {
    int row, col, dist;
    State(int r, int c, int d) {
      row = r; col = c; dist = d;
    }
  }

  public static int mazeRunner(int n, int m, int maze[][], int start[], int destination[]) {
    int[][] dist = new int[n][m];
    for (int[] row : dist) Arrays.fill(row, Integer.MAX_VALUE);

    int[] dr = {1, -1, 0, 0};
    int[] dc = {0, 0, 1, -1};

    PriorityQueue<State> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a.dist));
    pq.add(new State(start[0], start[1], 0));
    dist[start[0]][start[1]] = 0;

    while (!pq.isEmpty()) {
      State cur = pq.poll();

      if (cur.row == destination[0] && cur.col == destination[1]) {
        return cur.dist;
      }

      if (cur.dist > dist[cur.row][cur.col]) continue;

      for (int d = 0; d < 4; d++) {
        int r = cur.row, c = cur.col, steps = 0;

        // roll until wall
        while (r + dr[d] >= 0 && r + dr[d] < n && c + dc[d] >= 0 && c + dc[d] < m
               && maze[r + dr[d]][c + dc[d]] == 0) {
          r += dr[d];
          c += dc[d];
          steps++;
        }

        int newDist = cur.dist + steps;
        if (newDist < dist[r][c]) {
          dist[r][c] = newDist;
          pq.add(new State(r, c, newDist));
        }
      }
    }

    return -1; // unreachable
  }
}
```

---

## Example Walkthrough

Input:

```
N=5, M=5
maze = 
0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

start = [0,4]
destination = [4,4]
```

Steps:

1. Start at (0,4). Roll left → (0,3). Roll down → (4,4).
2. Path found with distance = **12** (as explained in problem).

Answer = **12**.

---

