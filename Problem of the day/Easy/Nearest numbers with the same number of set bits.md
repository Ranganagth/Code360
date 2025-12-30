# Intuition

The binary form fixes how many 1-bits the answer must keep.
To get the **next larger** number, push one of the 1-bits **left** (to make the value bigger) and rearrange the remaining bits on the right to be the **smallest** possible.
To get the **previous smaller** number, pull one of the 1-bits **right** (to make the value smaller) and rearrange the remaining bits on the right to be the **largest** possible.

Both tasks reduce to locating specific “01” / “10” patterns in the binary string and then clearing and refilling the suffix with the required count of 1-bits.

---

# Approach

Let `n` have `c1` ones and `c0` zeros near the pattern we adjust.

Next greater (same set-bit count):

1. Count trailing zeros (`c0`) then the consecutive ones after them (`c1`).
2. Position `p = c0 + c1` is the right-most place where a `0` can be flipped to `1`.
3. Set bit `p`, clear all bits right of `p`, then place `(c1-1)` ones at the far right.

Previous smaller (same set-bit count):

1. Count trailing ones (`c1`) then the consecutive zeros after them (`c0`).
2. Position `p = c0 + c1` is the right-most place where a `1` can be flipped to `0`.
3. Clear bits from `p` downward, then place `(c1+1)` ones just left-aligned inside that cleared suffix.

Both operations exist within 30 bits per constraints.

---

# Complexity

- **Time:** O(1)
- **Space:** O(1)

---

# Code

```java
import java.util.*;

public class Solution {

    private static int getNext(int n) {
        int c = n;
        int c0 = 0, c1 = 0;

        // count trailing zeros
        while (((c & 1) == 0) && (c != 0)) {
            c0++;
            c >>= 1;
        }
        // count ones just after trailing zeros
        while ((c & 1) == 1) {
            c1++;
            c >>= 1;
        }

        int p = c0 + c1;               // position to flip

        // flip rightmost non-trailing zero
        n |= (1 << p);

        // clear all bits to the right of p
        n &= ~((1 << p) - 1);

        // insert (c1 - 1) ones on the right
        n |= (1 << (c1 - 1)) - 1;

        return n;
    }

    private static int getPrev(int n) {
        int temp = n;
        int c0 = 0, c1 = 0;

        // count trailing ones
        while ((temp & 1) == 1) {
            c1++;
            temp >>= 1;
        }
        // count zeros just after trailing ones
        while (((temp & 1) == 0) && (temp != 0)) {
            c0++;
            temp >>= 1;
        }

        int p = c0 + c1;              // position to flip

        // clear from bit p down to 0
        n &= (~0) << (p + 1);

        // sequence of (c1 + 1) ones
        int mask = (1 << (c1 + 1)) - 1;

        // shift to put them just left of p
        n |= mask << (c0 - 1);

        return n;
    }

    public static ArrayList<Integer> nearestNumbers(int n) {
        ArrayList<Integer> res = new ArrayList<>();
        int next = getNext(n);
        int prev = getPrev(n);
        res.add(next);
        res.add(prev);
        return res;
    }
}
```

---


# Example walkthrough

**n** = 6 → binary 0110, set-bits = 2

**Next:**

* trailing zeros c0 = 1, ones after that c1 = 2 → p = 3
* flip bit 3: 1110
* clear right side: 1000
* add (c1-1)=1 one on right → 1001 = 9

**Previous:**

* trailing ones c1 = 0, zeros after that c0 = 1 → p = 1
* clear from p down: 0100
* add (c1+1)=1 one at position (c0-1)=0 → 0101 = 5

**Answer:** {9, 5}
