# Intuition

The middle value is the one that is neither the maximum nor the minimum. With three distinct numbers, this can be determined using comparisons only, without sorting.

---

# Approach (minimum comparisons)

Check relative ordering directly:

* If `x` lies between `y` and `z`, it is the answer.
* Else if `y` lies between `x` and `z`, it is the answer.
* Otherwise, `z` is the middle.

This uses only logical comparisons and avoids extra structures.

**Logic condition**
A number `a` is in the middle of `b` and `c` if:
`(b < a && a < c) || (c < a && a < b)`

---

# Complexity

- **Time Complexity:** O(1)
- **Space Complexity:** O(1)

---

# Code

```java
public class Solution {
    public static int middleOfThree(int x, int y, int z) {

        if ((y < x && x < z) || (z < x && x < y)) {
            return x;
        }

        if ((x < y && y < z) || (z < y && y < x)) {
            return y;
        }

        return z;
    }
};

```

---

# Example Walkthrough

**Input:** `x = 15, y = 2, z = 3`

* Check `x`: `2 < 15 < 3` → false
* Check `y`: `15 < 2 < 3` or `3 < 2 < 15` → false
* Remaining value is `z = 3`

**Output:** `3`
