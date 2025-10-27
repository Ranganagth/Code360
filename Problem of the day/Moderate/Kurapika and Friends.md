# Intuition

Each friend is available between `s[i]` and `e[i]` (inclusive).
We need to find **a single day** where the maximum number of friends overlap (i.e., are available on that day).

This is a **classic interval overlap problem** — similar to “maximum meetings overlapping” or “maximum number of guests in a hotel at once”.

---

# Approach (Sweep Line Algorithm)

1. We want to find the **maximum overlap** of intervals.
2. Instead of checking every possible day (which could be up to `10^9`), we focus only on **change points** — start and end days.

### Steps:

1. Create two arrays:

   * `s[]`: start times (when availability begins)
   * `e[]`: end times (when availability ends)

2. Sort both arrays individually.

3. Use **two pointers**:

   * `i` for start times
   * `j` for end times

4. Maintain:

   * `curr` = current number of overlapping friends
   * `maxFriends` = maximum overlap seen so far

5. Sweep through the timeline:

   * If `s[i] <= e[j]`:
     ⇒ A new friend became available before the previous one became unavailable
     ⇒ `curr++`, move `i++`
   * Else:
     ⇒ A friend’s availability ended
     ⇒ `curr--`, move `j++`
   * Keep updating `maxFriends = max(maxFriends, curr)`

6. Return `maxFriends`.

---

# Complexity

* Sorting: `O(N log N)`
* Sweep traversal: `O(N)`
* **Total:** `O(N log N)`
* **Space:** `O(1)` (ignoring input arrays)

This is efficient for `N ≤ 5000`.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int maxFriends(int n, int[] s, int[] e) {
        Arrays.sort(s);
        Arrays.sort(e);

        int i = 0, j = 0;
        int curr = 0, maxFriends = 0;

        while (i < n && j < n) {
            if (s[i] <= e[j]) {
                curr++;
                maxFriends = Math.max(maxFriends, curr);
                i++;
            } else {
                curr--;
                j++;
            }
        }

        return maxFriends;
    }
};

```

---

## Example Walkthrough

**Example:**
`s = [1, 4, 11]`
`e = [9, 5, 13]`

### Step 1: Sort both

`s = [1, 4, 11]`
`e = [5, 9, 13]`

### Step 2: Initialize

`i = 0, j = 0, curr = 0, maxFriends = 0`

### Step 3: Traverse

| Step              | Compare    | Action | curr | max |
| ----------------- | ---------- | ------ | ---- | --- |
| s[0]=1 ≤ e[0]=5   | new friend | curr=1 | 1    |     |
| s[1]=4 ≤ e[0]=5   | new friend | curr=2 | 2    |     |
| s[2]=11 > e[0]=5  | end friend | curr=1 | 2    |     |
| s[2]=11 > e[1]=9  | end friend | curr=0 | 2    |     |
| s[2]=11 ≤ e[2]=13 | new friend | curr=1 | 2    |     |

Result = `2`

---

## Example Verification

### Input:

```
n = 3
s = [1, 4, 11]
e = [9, 5, 13]
```

### Output:

```
2
```

Matches expected answer.

---

