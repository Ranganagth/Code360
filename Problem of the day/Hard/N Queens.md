# Intuition

Each row must contain exactly one queen.
Conflict constraints:

* Same column
* Same diagonal (both directions)

Use **backtracking**:

* Place queens row by row
* Validate before placing
* Backtrack when invalid

---

# Approach

1. Maintain:

   * `col[]` → columns occupied
   * `diag1[]` → (row + col)
   * `diag2[]` → (row - col + n - 1)
2. For each row:

   * Try placing queen in every column
   * If safe:

     * mark
     * recurse to next row
     * unmark (backtrack)
3. When row == n:

   * store configuration (flattened matrix)

---

# Complexity

* **Time complexity:**
  $$O(N!)$$

* **Space complexity:**
  $$O(N^2)$$ (for storing solutions)

---

# Code

```Java
import java.util.*;

public class Solution {
    public static ArrayList<ArrayList<Integer>> solveNQueens(int n) {

        ArrayList<ArrayList<Integer>> result = new ArrayList<>();

        boolean[] col = new boolean[n];
        boolean[] diag1 = new boolean[2 * n];
        boolean[] diag2 = new boolean[2 * n];

        int[][] board = new int[n][n];

        backtrack(0, n, board, col, diag1, diag2, result);

        return result;
    }

    private static void backtrack(int row, int n, int[][] board,
                                 boolean[] col, boolean[] diag1, boolean[] diag2,
                                 ArrayList<ArrayList<Integer>> result) {

        if (row == n) {
            ArrayList<Integer> config = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    config.add(board[i][j]);
                }
            }
            result.add(config);
            return;
        }

        for (int c = 0; c < n; c++) {

            if (col[c] || diag1[row + c] || diag2[row - c + n - 1]) continue;

            board[row][c] = 1;
            col[c] = true;
            diag1[row + c] = true;
            diag2[row - c + n - 1] = true;

            backtrack(row + 1, n, board, col, diag1, diag2, result);

            board[row][c] = 0;
            col[c] = false;
            diag1[row + c] = false;
            diag2[row - c + n - 1] = false;
        }
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

N = 4

Valid configurations:

Config 1:
0 0 1 0
1 0 0 0
0 0 0 1
0 1 0 0

Config 2:
0 1 0 0
0 0 0 1
1 0 0 0
0 0 1 0

Flattened outputs correspond to these boards

---

## Example 2:

N = 3

No valid placements exist

Result = empty list
