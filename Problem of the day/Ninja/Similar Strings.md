# Intuition

For each rotation of `A`, compute:
$$
\sum |A[i] - B[i]| + |A[i] - C[i]|
$$

Brute force:

* Try all rotations → (O(N))
* Each rotation compute full sum → (O(N))

Total:
$$
O(N^2)
$$
Given $(N \le 10^4)$, this passes.

---

# Approach

1. For each rotation `r` from `0 → n-1`:

   * Compare:

     * `A[(i + r) % n]` with `B[i]` and `C[i]`
2. Compute total difference
3. Track:

   * `min`
   * `max`
4. Return both

---

# Complexity

* **Time complexity:**
  $$O(N^2)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {
    
    public static int[] similarStrings(int n, String a, String b, String c) {

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for (int r = 0; r < n; r++) {

            int total = 0;

            for (int i = 0; i < n; i++) {

                char chA = a.charAt((i + r) % n);
                char chB = b.charAt(i);
                char chC = c.charAt(i);

                total += Math.abs(chA - chB);
                total += Math.abs(chA - chC);
            }

            min = Math.min(min, total);
            max = Math.max(max, total);
        }

        return new int[]{max, min};
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

A = "abc"
B = "aaa"
C = "bba"

Rotations:

r = 0 → "abc"
diff = 6

r = 1 → "bca"
diff = 4

r = 2 → "cab"
diff = 6

Result:
max = 6
min = 4

---

## Example 2:

A = "ab"
B = "bb"
C = "ab"

r = 0 → "ab" → diff = 3
r = 1 → "ba" → diff = 1

Result:
max = 3
min = 1
