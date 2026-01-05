# Intuition

This is the classic “ant” simulation. The ant stands on a cell, looks at its value, rotates, flips the cell, then moves forward. Repeat for the given number of moves. If at any point the move sends it outside the matrix, terminate and return `-1 -1`.

**Rules distilled**
Cell value = 1 → rotate right, flip to 0, move forward
Cell value = 0 → rotate left, flip to 1, move forward
Initial direction = east
Directions cycle: north(0,−1), east(1,0), south(0,1), west(−1,0) expressed as row/col deltas.

**Rotation math**
Store directions in order: up, right, down, left.
Right turn → `(dir + 1) % 4`
Left turn → `(dir + 3) % 4`

After updating direction, flip the cell, then move.

**Stop condition**
If the new row or column goes out of bounds, return `[-1, -1]`.

---

# Complexity

- **Time:** `O(moves)`
- **Space:** `O(1)` extra (matrix modified in-place)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<Integer> ninjaAnt(int[][] mat, int sr, int sc, int moves) {

        int n = mat.length;

        // directions: up, right, down, left
        int[][] dirs = { {-1,0}, {0,1}, {1,0}, {0,-1} };
        int dir = 1; // initially facing east (index 1)

        int r = sr, c = sc;

        for (int step = 0; step < moves; step++) {

            if (mat[r][c] == 1) {
                dir = (dir + 1) % 4;   // turn right
                mat[r][c] = 0;        // flip
            } else {
                dir = (dir + 3) % 4;   // turn left
                mat[r][c] = 1;        // flip
            }

            r += dirs[dir][0];
            c += dirs[dir][1];

            if (r < 0 || r >= n || c < 0 || c >= n) {
                ArrayList<Integer> out = new ArrayList<>();
                out.add(-1);
                out.add(-1);
                return out;
            }
        }

        ArrayList<Integer> ans = new ArrayList<>();
        ans.add(r);
        ans.add(c);
        return ans;
    }
};

```

---

# Walkthrough

**Matrix**
1 1
0 0

start=(0,0), moves=1

**Step 1:** 
value=1 → turn right (east→south), flip to 0, move to (1,0).

Inside bounds, stop.
Result: `[1, 0]`
