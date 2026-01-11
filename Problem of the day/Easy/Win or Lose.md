# Intuition

The game outcome depends only on the **parity of N**. Every move subtracts a **proper divisor i** of N. Since `1` always divides N (for N > 1), a move always flips parity:

* even → odd
* odd → even

The losing state is `N = 1` because no valid `i` exists. Work backward: if you give your opponent `1`, they lose. So any position that can force the opponent to `1` is winning.

---

# Approach

Observe the pattern:

* `N = 1` → lose
* `N = 2` → win (2 − 1 = 1)
* `N = 3` → lose (3 − 1 = 2 → opponent wins)
* `N = 4` → win
* `N = 5` → lose

This alternates strictly by parity.

**Rule:**

* If `N` is **even** → first player wins → `"YES"`
* If `N` is **odd** → first player loses → `"NO"`

No simulation. No DP. No factorization.

---

# Complexity

- **Time:** `O(1)` per test case
- **Space:** `O(1)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static String winOrLose(int N) {
        if (N % 2 == 0) return "YES";
        return "NO";
    }
};

```

---

# Example walkthrough

**Case 1:** `N = 2`

* You choose `i = 1` → `N = 1`
* Opponent has no move → you win

**Case 2:** `N = 3`

* You choose `i = 1` → `N = 2`
* Opponent chooses `i = 1` → `N = 1`
* You have no move → you lose

**Case 3:** `N = 16`

* Even number → winning state immediately by parity rule

**Case 4:** `N = 13`

* Odd number → losing state immediately by parity rule

---
