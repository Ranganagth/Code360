# Intuition

The robot:

* Starts at `(0,0)` facing **north**.
* Turns left/right based on commands `-2` and `-1`.
* Moves forward based on commands `1..9`.
* Obstacles block its path (the robot stays in place if the next step is an obstacle).
* We need to track the **maximum Euclidean distance squared** from the origin.

---

# Approach

1. **Direction tracking**

   * Use an array for directions:

     ```
     north = (0,1), east = (1,0), south = (0,-1), west = (-1,0)
     ```

     i.e. `dirs = {{0,1},{1,0},{0,-1},{-1,0}}`
   * Maintain `dirIdx = 0` (0 = north, 1 = east, etc.).

2. **Obstacle set**

   * Store obstacles in a `HashSet<String>` like `"x,y"` for `O(1)` lookup.

3. **Processing commands**

   * If command is `-2`, turn left: `dirIdx = (dirIdx + 3) % 4`.
   * If command is `-1`, turn right: `dirIdx = (dirIdx + 1) % 4`.
   * Else, move step by step up to `K` units:

     * For each step, compute `nx, ny`.
     * If not an obstacle, update position.
     * Else, break from that move.
   * After each move, compute distance squared = `x*x + y*y` and update max.

4. **Return result**

---

# Complexity

* **Time:** `O(C + K)` where `C = number of commands`, `K = number of steps walked`.
* **Space:** `O(M)` where `M = number of obstacles`.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int robotSim(ArrayList<Integer> commands, int[][] obstacles) {
        // Directions: north, east, south, west
        int[][] dirs = {{0,1},{1,0},{0,-1},{-1,0}};
        int dirIdx = 0; // facing north

        // Store obstacles in a set for O(1) lookup
        Set<String> obsSet = new HashSet<>();
        for (int[] obs : obstacles) {
            obsSet.add(obs[0] + "," + obs[1]);
        }

        int x = 0, y = 0;
        int maxDistSq = 0;

        for (int cmd : commands) {
            if (cmd == -2) { 
                // turn left
                dirIdx = (dirIdx + 3) % 4;
            } else if (cmd == -1) {
                // turn right
                dirIdx = (dirIdx + 1) % 4;
            } else {
                // move forward step by step
                for (int step = 0; step < cmd; step++) {
                    int nx = x + dirs[dirIdx][0];
                    int ny = y + dirs[dirIdx][1];
                    if (obsSet.contains(nx + "," + ny)) {
                        // obstacle -> stop this move
                        break;
                    }
                    x = nx;
                    y = ny;
                    maxDistSq = Math.max(maxDistSq, x*x + y*y);
                }
            }
        }
        return maxDistSq;
    }
}
```

---

### Example Walkthrough

Input:

```
commands = [4,-1,3]
obstacles = [[2,1]]
```

* Start: `(0,0)`, facing north.
* Command 4: move north 4 steps → `(0,4)`.
* Command -1: turn right (now facing east).
* Command 3: try to move east → `(1,4)`, `(2,4)`, `(3,4)` (no obstacle).

Max distance squared = `(3^2 + 4^2) = 25`.

Output = `25`.

---
