# Intuition

A leader is an element that is strictly greater than everything to its right. Scanning left-to-right is expensive because each element would need to compare against all to its right. Scanning right-to-left solves it: track the maximum seen so far. Anything greater than that running maximum is a leader.

---

# Approach

Start from the last index.
Maintain `maxRight = -∞`.
For each element from right to left:

* If current > `maxRight`, mark it as a leader and update `maxRight`.
  Store leaders while scanning right-to-left, then reverse them to restore original order.

---

# Complexity

- **Time:** `O(N)`
- **Space:** `O(N)` (for the leaders list; `O(1)` extra beyond output)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<Integer> findLeaders(ArrayList<Integer> elements, int n) {

        ArrayList<Integer> leaders = new ArrayList<>();
        long maxRight = Long.MIN_VALUE;

        for (int i = n - 1; i >= 0; i--) {
            int val = elements.get(i);
            if (val > maxRight) {
                leaders.add(val);
                maxRight = val;
            }
        }

        Collections.reverse(leaders);
        return leaders;
    }
};

```

---

# Example walkthrough

**Input:** `[6, 7, 4, 2, 5, 3]`

**Scan from right:**

* 3 → leader, maxRight=3
* 5 → leader, maxRight=5
* 2 → skip
* 4 → skip
* 7 → leader, maxRight=7
* 6 → skip

**Collected** (reverse order while scanning): `[3, 5, 7]`
Reverse to match original sequence order → **`7 5 3`**
