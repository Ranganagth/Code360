# Intuition

* This game eliminates one player in each round after passing the ball `K` times.
* The elimination step matches the **Josephus elimination process**:

  * In Josephus, every `K`-th person is eliminated from the circle until one remains.
* So the winner’s position is **exactly the Josephus(n, k)** solution.

The Josephus recurrence:

$$
J(1, k) = 0
$$

$$
J(n, k) = (J(n-1, k) + k) \mod n
$$

* This recurrence computes the **0-based index** of the winner.
* Since in your problem, players are **1-based labeled (1 to N)**, we return:

$$
\text{Winner} = J(n, k) + 1
$$

---

# Approach

1. Use the iterative form of the Josephus recurrence (to avoid recursion stack issues with large `n`).
2. Start with `res = 0` (base case).
3. Loop from `i = 2` to `n`, and update:

   $$
   res = (res + k) \mod i
   $$
4. At the end, return `res + 1` (to convert to 1-based answer).

---

# Complexity

* **Time:** `O(n)` per test case (since we iterate once through all players).
* **Space:** `O(1)` (only a few variables).

This works within the constraints:

* `n ≤ 10^5`, `T ≤ 10`.
* `O(n * T) = 10^6` operations max → efficient.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int predictTheWinner(int n, int k) {
        int res = 0; // Josephus base case for 1 person
        for (int i = 2; i <= n; i++) {
            res = (res + k) % i;
        }
        return res + 1; // convert to 1-based
    }
}
```

---

### Example Walkthrough

#### Input:

```
N = 5, K = 2
```

* Step 1: `res = 0`
* i = 2 → res = (0 + 2) % 2 = 0
* i = 3 → res = (0 + 2) % 3 = 2
* i = 4 → res = (2 + 2) % 4 = 0
* i = 5 → res = (0 + 2) % 5 = 2

Final answer: `res + 1 = 3`. 

---
