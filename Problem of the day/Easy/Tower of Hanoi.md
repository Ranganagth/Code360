# **Problem: Tower of Hanoi**

You are given `N` disks on **rod 1** arranged in increasing size from top to bottom. The goal is to move all disks to **another rod (2 or 3)** by following these rules:

1. **Only one disk can be moved at a time.**
2. **Only the top disk can be moved from any rod.**
3. **A larger disk cannot be placed on top of a smaller disk.**
4. **Return the sequence of moves.**

---

# Intuition

The Tower of Hanoi problem is a **classic recursive problem**. The key idea is to:

* Break the problem of `N` disks into smaller subproblems.
* Move `N-1` disks to an auxiliary rod to free up the largest disk.
* Move the largest disk to the destination.
* Then move the `N-1` disks over to the destination.

This pattern keeps repeating for smaller numbers until we move a single disk.

---

# Approach

We use **recursion**. Let's say we want to move `n` disks from `src` to `dest`, using `aux`:

1. Recursively move `n-1` disks from `src` to `aux`.
2. Move the largest (nth) disk from `src` to `dest`.
3. Recursively move the `n-1` disks from `aux` to `dest`.

### Base Case:

* If `n == 0`, there are no disks to move. So just return.

We’ll use an `ArrayList<ArrayList<Integer>>` to store each move as a pair `[from, to]`.

---

# Complexity Analysis

| Metric           | Value                                                         |
| ---------------- | ------------------------------------------------------------- |
| Time Complexity  | `O(2^N)` – because each call splits into two recursive calls. |
| Space Complexity | `O(2^N)` – for storing the result list and recursion stack.   |

---
# Code

```java
import java.util.ArrayList;

public class Solution {
    public static ArrayList<ArrayList<Integer>> towerOfHanoi(int n) {
        ArrayList<ArrayList<Integer>> moves = new ArrayList<>();
        solve(n, 1, 3, 2, moves);  // move from rod 1 to rod 3 using rod 2
        return moves;
    }

    private static void solve(int n, int src, int dest, int aux, ArrayList<ArrayList<Integer>> moves) {
        if (n == 0) return;

        // Step 1: Move top n-1 disks from src to aux
        solve(n - 1, src, aux, dest, moves);

        // Step 2: Move the nth disk from src to dest
        ArrayList<Integer> move = new ArrayList<>();
        move.add(src);
        move.add(dest);
        moves.add(move);

        // Step 3: Move the n-1 disks from aux to dest
        solve(n - 1, aux, dest, src, moves);
    }
}
```

---
## Example Walkthrough

Let’s walk through `n = 2`:

Initial Rods:

```
Rod 1: [2, 1]  
Rod 2: []  
Rod 3: []
```

### Steps:

1. Move `1` from rod 1 to rod 2 → Rod 2: \[1]
2. Move `2` from rod 1 to rod 3 → Rod 3: \[2]
3. Move `1` from rod 2 to rod 3 → Rod 3: \[2, 1]

Moves:

```
[1, 2]
[1, 3]
[2, 3]
```

---

## Sample Input/Output

**Input:**

```
1
3
```

**Output:**

```
[1, 3]
[1, 2]
[3, 2]
[1, 3]
[2, 1]
[2, 3]
[1, 3]
```
