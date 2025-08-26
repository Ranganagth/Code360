# Intuition

* Ninja (`X`) always plays first.
* Friend (`O`) plays second.
* We want to compute the **best score Ninja can guarantee** by playing optimally.
* At every move:

  * If it’s Ninja’s turn → maximize the score.
  * If it’s Friend’s turn → minimize the score.

We explore all possible moves recursively until the game ends, and propagate scores back using **Minimax**.

---

# Approach

1. **Check terminal state**:

   * If `X` wins → return score `+1`.
   * If `O` wins → return score `-1`.
   * If board full → return score `0` (draw).

2. **Recursive minimax**:

   * If it’s Ninja’s turn (`X`):
     Try all empty cells → place `X` → recurse → undo.
     Pick max score.
   * If it’s Friend’s turn (`O`):
     Try all empty cells → place `O` → recurse → undo.
     Pick min score.

3. **Finding best move**:

   * For Ninja’s turn: among all moves, pick the one that gives the **best score**.
   * If multiple moves yield same score → choose smallest row, then smallest column.

---

# Complexity

* Branching factor ≤ 9 (empty cells).
* Depth ≤ 9.
* Worst case complexity ≈ `O(9!) ≈ 362,880` (manageable since grid = 3x3).
* Efficient enough for this problem.

---

# Code

```java
import java.util.*;

public class Solution {
    
    // Check if there is a winner
    private static int evaluate(char[][] board) {
        // Rows
        for (int i = 0; i < 3; i++) {
            if (board[i][0] != '_' && board[i][0] == board[i][1] && board[i][1] == board[i][2]) {
                return (board[i][0] == 'X') ? 1 : -1;
            }
        }
        // Columns
        for (int j = 0; j < 3; j++) {
            if (board[0][j] != '_' && board[0][j] == board[1][j] && board[1][j] == board[2][j]) {
                return (board[0][j] == 'X') ? 1 : -1;
            }
        }
        // Diagonals
        if (board[0][0] != '_' && board[0][0] == board[1][1] && board[1][1] == board[2][2]) {
            return (board[0][0] == 'X') ? 1 : -1;
        }
        if (board[0][2] != '_' && board[0][2] == board[1][1] && board[1][1] == board[2][0]) {
            return (board[0][2] == 'X') ? 1 : -1;
        }
        
        return 0; // No winner yet
    }
    
    private static boolean isMovesLeft(char[][] board) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == '_') return true;
            }
        }
        return false;
    }

    // Minimax function
    private static int minimax(char[][] board, boolean isXturn) {
        int score = evaluate(board);
        if (score != 0) return score; // someone already won
        if (!isMovesLeft(board)) return 0; // draw

        if (isXturn) {
            int best = Integer.MIN_VALUE;
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (board[i][j] == '_') {
                        board[i][j] = 'X';
                        best = Math.max(best, minimax(board, false));
                        board[i][j] = '_'; // undo
                    }
                }
            }
            return best;
        } else {
            int best = Integer.MAX_VALUE;
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (board[i][j] == '_') {
                        board[i][j] = 'O';
                        best = Math.min(best, minimax(board, true));
                        board[i][j] = '_'; // undo
                    }
                }
            }
            return best;
        }
    }
    
    public static int[] tictactoeWinner(char[][] board) {
        int bestScore = Integer.MIN_VALUE;
        int bestRow = -1, bestCol = -1;

        // Try all possible moves for X
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == '_') {
                    board[i][j] = 'X';
                    int moveScore = minimax(board, false);
                    board[i][j] = '_'; // undo
                    
                    if (moveScore > bestScore || 
                       (moveScore == bestScore && (i < bestRow || (i == bestRow && j < bestCol)))) {
                        bestScore = moveScore;
                        bestRow = i;
                        bestCol = j;
                    }
                }
            }
        }
        
        // Return {score, row+1, col+1}
        return new int[]{bestScore, bestRow + 1, bestCol + 1};
    }
}
```

---

## Walkthrough Example

Input:

```
X O X
O O X
_ _ _
```

* Try placing `X` at (3,3) → immediate win → score = 1
* Any other cell leads to draw or worse.
  So result:

```
1 3 3
```

---

