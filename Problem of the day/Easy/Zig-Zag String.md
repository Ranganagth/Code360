# Intuition

You must simulate how characters land when you write the string vertically down across `m` rows, then diagonally up to row 0, then down again, repeating. Each row collects the characters that fall into it. Finally, concatenate the rows.

---

# Approach

If `m == 1`, the pattern is just the original string.
Otherwise, create `m` builders (one per row). Traverse the string once while tracking:

* current row index
* direction: moving down or up

Append each character to its row.
When you hit the top row, flip direction to “down”.
When you hit the bottom row, flip direction to “up”.

After processing all characters, join the rows.

---
# Complexity

- **Time:** `O(N)` - one pass over the string
- **Space:** `O(N)` - storing characters spread across rows

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String zigZagString(String str, int n, int m) {

        if (m == 1 || m >= n) return str;

        StringBuilder[] rows = new StringBuilder[m];
        for (int i = 0; i < m; i++) rows[i] = new StringBuilder();

        int row = 0;
        int dir = 1; // +1 means moving down, -1 means moving up

        for (int i = 0; i < n; i++) {
            rows[row].append(str.charAt(i));

            if (row == 0) dir = 1;
            else if (row == m - 1) dir = -1;

            row += dir;
        }

        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < m; i++) ans.append(rows[i]);

        return ans.toString();
    }
};

```

---

# Walkthrough example

**Input:** `STR = "ABCDEFG", N = 7, M = 3`

## Row trackers start empty:

**Row0:**
**Row1:**
**Row2:**

## Process characters:

A → row0
**Row0:** A

B → row1
**Row1:** B

C → row2 (bottom reached → change direction up)
**Row2:** C

D → row1
**Row1:** BD

E → row0 (top reached → change direction down)
**Row0:** AE

F → row1
**Row1:** BDF

G → row2
**Row2:** CG

**Concatenate rows:** `AE` + `BDF` + `CG` → `AEBDFCG`
