# Intuition

This is a **two-player impartial game**.
A position is **losing** if the current player has **no move that forces the opponent into a losing position**.
A position is **winning** if **at least one move leads to a losing position for the opponent**.

Allowed moves from `N`:

* Remove **1 stone**
* Remove **any even number of stones**

Ale always starts.

---
# Approach
### Key Observation (Game Theory)

Let’s analyze small values and detect the pattern.

| Stones (N) | Result  | Reason                   |
| ---------- | ------- | ------------------------ |
| 0          | Losing  | No move possible         |
| 1          | Winning | Take 1                   |
| 2          | Winning | Take 2                   |
| 3          | Losing  | Any move → opponent wins |
| 4          | Winning | Take all                 |
| 5          | Winning | Take 2 → leaves 3        |
| 6          | Winning | Take all                 |
| 7          | Winning | Take 4 → leaves 3        |
| 8          | Winning | Take all                 |
| ...        | ...     | ...                      |

### Crucial Pattern

* **Only `N = 3` is a losing position**
* Every other `N ≥ 1` is winning for the first player

Reason:
For any `N > 3`, Ale can always remove an even number to **force Bob to face `3`**, which is losing.

### Final Rule

```text
If N == 3 → Bob wins
Else → Ale wins
```

---

# Complexity

* **Time:** `O(1)` per test case
* **Space:** `O(1)`

Handles up to `10^5` test cases effortlessly.

---

# Code

```java
public class Solution {

    public static String gameOfStones(int num) {
        if (num == 3) {
            return "Bob";
        }
        return "Ale";
    }
};

```

---

# Example Walkthrough

## Example 1
#### Input

```
3
```

* Ale can remove `1` or `2`
* Both lead to positions where Bob can take the remaining stones
* Ale loses

**Output:** `Bob`

## Example 2
#### Input

```
6
```

* Ale removes all 6 stones (even number)
* Bob has no move

**Output:** `Ale`

---

### Final Conclusion

The game collapses to a **single losing state**.

> **Ale loses if and only if the number of stones is exactly `3`.**

Everything else is a guaranteed win for Ale.
