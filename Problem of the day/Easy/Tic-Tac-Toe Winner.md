# Intuition

The core idea is to simulate the Tic-Tac-Toe game by marking moves on a 3x3 grid and checking after each move whether a player has won. The game ends either when:

* A player wins by marking three in a row (row, column, or diagonal),
* All positions are filled without a winner (draw),
* Or moves are pending but no winner yet (uncertain).

---

# Approach

1. Use a 3x3 integer grid to simulate the board.
2. Iterate over the `moves` list and alternate turns between player1 (uses `1`) and player2 (uses `2`).
3. After all moves are placed:
   * Check all rows, columns, and diagonals for a winner.
4. If a winner is found, return `"player1"` or `"player2"` accordingly.
5. If no winner but total moves < 9, return `"uncertain"`.
6. If all 9 moves are made with no winner, return `"draw"`.

---

# Complexity

* **Time complexity:**
  $O(n + 1)$
  Checking win conditions is constant since the board is fixed at 3x3.

* **Space complexity:**
  $O(1)$
  Only a fixed-size grid (3x3) is used regardless of the number of moves.

---

# Code

```java
import java.util.ArrayList;

public class Solution {
    public static String ticTacToeWinner(ArrayList<ArrayList<Integer>> moves, int n) {
        int[][] board = new int[3][3]; // 0: empty, 1: player1(X), 2: player2(O)

        for (int i = 0; i < n; i++) {
            int row = moves.get(i).get(0);
            int col = moves.get(i).get(1);
            int player = (i % 2 == 0) ? 1 : 2;
            board[row][col] = player;
        }

        // Check rows and columns
        for (int i = 0; i < 3; i++) {
            if (board[i][0] != 0 &&
                board[i][0] == board[i][1] &&
                board[i][1] == board[i][2]) {
                return board[i][0] == 1 ? "player1" : "player2";
            }

            if (board[0][i] != 0 &&
                board[0][i] == board[1][i] &&
                board[1][i] == board[2][i]) {
                return board[0][i] == 1 ? "player1" : "player2";
            }
        }

        // Check diagonals
        if (board[0][0] != 0 &&
            board[0][0] == board[1][1] &&
            board[1][1] == board[2][2]) {
            return board[0][0] == 1 ? "player1" : "player2";
        }

        if (board[0][2] != 0 &&
            board[0][2] == board[1][1] &&
            board[1][1] == board[2][0]) {
            return board[0][2] == 1 ? "player1" : "player2";
        }

        // No winner found
        return n < 9 ? "uncertain" : "draw";
    }
}
```

---

### **Example Walkthrough**

### Input:

```
moves = [[0,0], [1,1], [0,2], [2,2], [2,1]]
```

* player1: (0,0), (0,2), (2,1)
* player2: (1,1), (2,2)

**Board after moves:**

```
X _ X
_ O _
_ X O
```

* No row, column, or diagonal with 3 same marks
* Total moves = 5 < 9 → Remaining moves possible

**Output:** `"uncertain"`

---
