# Intuition

Prevent any run of length three. Always place the character that still has the larger remaining count, but cap consecutive placements at two. Alternate when needed.

---

# Approach

Track remaining zeros `m`, ones `n`, and the last two characters of the result.
Rule:

* If one character has more remaining count, place it unless it forms a triple.
* If placing it forms a triple, place the opposite character.
  Since the problem guarantees feasibility, this greedy always succeeds.

---

# Complexity

* **Time complexity:** O(m+n)
* **Space complexity:** O(m+n)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String strThree0Three1(int m, int n) {
        StringBuilder sb = new StringBuilder();
        
        int last1 = -1, last2 = -1;  // track last two chars: 0 or 1
        
        while (m > 0 || n > 0) {
            boolean putZero = false;

            if (m > n) putZero = true;
            else if (n > m) putZero = false;
            else if (m == n) putZero = (last1 != 0 || last2 != 0);  // avoid triple

            if (putZero) {
                if (last1 == 0 && last2 == 0) putZero = false;
            } else {
                if (last1 == 1 && last2 == 1) putZero = true;
            }

            if (putZero) {
                sb.append('0');
                last2 = last1;
                last1 = 0;
                m--;
            } else {
                sb.append('1');
                last2 = last1;
                last1 = 1;
                n--;
            }
        }
        
        return sb.toString();
    }
};

```

---

# Example walkthrough with explanation

Case: m = 4, n = 2
Start: counts unequal → place 0 until two zeros in a row → alternate with 1 to break runs.
Result pattern matches constraints: no triple zeros, no triple ones, total zeros = 4, ones = 2.
