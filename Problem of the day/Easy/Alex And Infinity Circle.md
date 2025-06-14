# Intuition

If Alex moves in a way such that:

1. He comes back to the **starting position (0,0)** after executing the string once, or
2. He doesn't return to (0,0) but **his direction changes** after one full execution (i.e., he's not facing north anymore),

then repeating the instructions will eventually bring him back to (0,0) — like moving in a loop.

However, if **he doesn’t return to (0,0)** and **still faces north**, then he’s just moving in a straight line away from the origin — **he’ll never return**.

---

# Approach

1. Initialize position `(x, y)` at `(0, 0)` and face `north`.
2. Define directions as: north `(0,1)`, east `(1,0)`, south `(0,-1)`, west `(-1,0)`.
3. Traverse the string once:

   * 'G': Move one unit in the current direction.
   * 'L': Turn left.
   * 'R': Turn right.
4. After processing the string:

   * If position is `(0,0)` → return `True`.
   * Else if the direction is not north → return `True`.
   * Else → return `False`.

---

# Complexity

* **Time complexity**: $O(n)$ per test case
* **Space complexity**: $O(1)$

---

# Code
## Solution 1

```java
import java.util.*;

public class Solution {
    public static String isPossible(String str, int n) {
        // Directions: North, East, South, West
        int[][] dirs = { {0,1}, {1,0}, {0,-1}, {-1,0} };
        int dir = 0; // Start facing north
        int x = 0, y = 0;

        for (int i = 0; i < n; i++) {
            char move = str.charAt(i);
            if (move == 'G') {
                x += dirs[dir][0];
                y += dirs[dir][1];
            } else if (move == 'L') {
                dir = (dir + 3) % 4; // turn left
            } else if (move == 'R') {
                dir = (dir + 1) % 4; // turn right
            }
        }

        // Return true if back to origin or facing not north
        if ((x == 0 && y == 0) || dir != 0) {
            return "True";
        } else {
            return "False";
        }
    }
}
```

---
## Solution 2

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static String isPossible(String str, int n) {
        int [][] direction = {{0,1}, {1,0}, {0, -1}, {-1, 0}};
        int x = 0, y = 0, dir = 0;

        for (char ch: str.toCharArray()) {
            switch (ch) {
                case 'G' -> {
                    x += direction[dir][0];
                    y += direction[dir][1];
                }
                case 'L' -> dir = (dir + 3) % 4;
                case 'R' -> dir = (dir + 1) % 4;
            }
        }
        return ((x == 0 && y == 00) || dir != 0) ? "True" : "False";
    }

    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            int T = scanner.nextInt();
            while (T-- > 0) {
                int N = scanner.nextInt();
                String str = scanner.next();
                System.out.println(isPossible(str, N));
            }
        }
    }
}

```
