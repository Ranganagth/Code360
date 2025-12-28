# Intuition

Each row is sorted. All 0s come before all 1s. If you know the position of the *first 1* in a row, you immediately know how many 1s it has. Walking row-by-row with binary search works, but scanning repeatedly wastes work. A better view: start at the **top-right**. If you see a 1, move **left** (more 1s in this row). If you see a 0, move **down** (this row has fewer 1s than possible below). This single pass covers at most `N + M` moves.

---

# Approach

1. Set `row = 0`, `col = M-1`.
2. Track `bestRow = -1`.
3. While inside bounds:

   * If `arr[row][col] == 1`: update `bestRow = row`, move `col--`.
   * Else: move `row++`.
4. Return `bestRow`.

## Why it works

Moving left means “this row has more 1s than any row seen so far”. Moving down discards rows that cannot beat the current best.

---

# Complexity

- **Time:** `O(N + M)`
- **Space:** `O(1)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int maxOne(ArrayList<ArrayList<Integer>> arr) {
        int n = arr.size();
        if (n == 0) return -1;
        int m = arr.get(0).size();

        int row = 0, col = m - 1;
        int bestRow = -1;

        while (row < n && col >= 0) {
            if (arr.get(row).get(col) == 1) {
                bestRow = row;
                col--;          // move left to find earlier 1s
            } else {
                row++;          // move down to a row that may have more 1s
            }
        }
        return bestRow;
    }
};

```

---

# Example walkthrough

**Matrix:**

```
0 0 1
1 1 1
0 1 1
```

Start `(0,2)=1` → bestRow=0 → left to `(0,1)=0` → down `(1,1)=1` → bestRow=1 → left `(1,0)=1` → bestRow=1 → left off column → stop.

Row 1 has the maximum count of 1s.
