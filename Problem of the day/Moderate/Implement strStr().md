# Intuition

Brute force matching gives $(O(n \cdot m))$, which is too slow.

Use **KMP (Knuth-Morris-Pratt)**:

* Avoid rechecking characters
* Precompute **LPS (Longest Prefix Suffix)** array
* Achieve linear time

---

# Approach

1. Build LPS array for pattern `A`

   * `lps[i]` = longest prefix which is also suffix till index `i`
2. Traverse string `B` using two pointers:

   * `i` for `B`
   * `j` for `A`
3. Matching logic:

   * If characters match → move both
   * If mismatch:

     * If `j > 0` → jump using LPS
     * Else → move `i`
4. If `j == A.length()` → match found → return index
5. If traversal ends → return -1

---

# Complexity

* **Time complexity:**
  $$O(n + m)$$

* **Space complexity:**
  $$O(m)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {

    public static int findIndexOf(String a, String b) {

        int m = a.length();
        int n = b.length();

        int[] lps = buildLPS(a);

        int i = 0, j = 0;

        while (i < n) {

            if (b.charAt(i) == a.charAt(j)) {
                i++;
                j++;
            }

            if (j == m) {
                return i - j;
            } else if (i < n && b.charAt(i) != a.charAt(j)) {

                if (j > 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }

        return -1;
    }

    private static int[] buildLPS(String pattern) {

        int m = pattern.length();
        int[] lps = new int[m];

        int len = 0;
        int i = 1;

        while (i < m) {

            if (pattern.charAt(i) == pattern.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {

                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }

        return lps;
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

A = "ninjas"
B = "codingninjas"

LPS for "ninjas" = [0,0,0,0,0,0]

Matching:

* Traverse B
* Match starts at index 6
* Full match found

Answer = 6

## Example 2:

A = "code"
B = "codingninjas"

Matching fails completely

Answer = -1
