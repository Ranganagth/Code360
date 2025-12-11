# Intuition

Maximize the size of the interval of C for which Andrew is strictly closer than Misha. That interval is always centered around Andrew and excludes the midpoint between Andrew and Misha. The farther Andrew is from Misha, the smaller his winning interval becomes. The maximum winning interval occurs when Andrew is the nearest integer just next to Misha (left or right).

---

# Approach

If Andrew picks a number A < M:
Winning interval: C < (A + M)/2.
Length grows as A approaches M from the left.

If Andrew picks A > M:
Winning interval: C > (A + M)/2.
Length grows as A approaches M from the right.

Thus, Andrew should pick the closest possible integer to Misha on either side:
If M > 1 → candidate A = M - 1.
If M < N → candidate A = M + 1.
Compute which side gives more winning positions.
The side that reaches farther before hitting array bounds wins.

Compute sizes:
LeftSize = M - 1
RightSize = N - M

Pick the side with larger size.
If equal → choose left (tie-breaking not stated, but optimality identical; left is deterministic).

---

# Complexity

- **Time:** O(1).
- **Space:** O(1).

---

# Code

```java
import java.util.*;

public class Solution {
    public static int minNumToWin(int n, int mishaNum) {
        int leftSize = mishaNum - 1;
        int rightSize = n - mishaNum;

        if (leftSize >= rightSize) return mishaNum - 1;
        else return mishaNum + 1;
    }
};

```

---

# Example Walkthrough

**Input:** (N = 4, M = 2)
leftSize = 1
rightSize = 2
Pick right → 3.

**Input:** (N = 3, M = 1)
leftSize = 0
rightSize = 2
Pick right → 2.
