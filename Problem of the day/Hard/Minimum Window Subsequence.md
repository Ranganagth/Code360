# Intuition

Requirement differs from classic minimum window:

* Window must be **substring of S**
* T must appear as a **subsequence inside that window**

Greedy matching alone fails because shrinking breaks subsequence order.

Use:

* Forward scan → find a valid window end
* Backward shrink → minimize window

---

# Approach

1. Iterate `i` over `S`
2. When `S[i] == T[0]`:

   * Start matching T forward:

     * Move pointer `j` in S
     * Move pointer `k` in T
   * If full T matched:

     * Now shrink from end:

       * Move backward to find smallest valid window
3. Track minimum length window

---

# Complexity

* **Time complexity:**
  $$O(N \cdot M)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
public class Solution {
    
    public static String minWindow(String S, String T) {

        int n = S.length();
        int m = T.length();

        int minLen = Integer.MAX_VALUE;
        int startIndex = -1;

        for (int i = 0; i < n; i++) {

            if (S.charAt(i) != T.charAt(0)) continue;

            int j = i;
            int k = 0;

            // forward match
            while (j < n && k < m) {
                if (S.charAt(j) == T.charAt(k)) {
                    k++;
                }
                j++;
            }

            // full match found
            if (k == m) {

                int end = j - 1;
                k = m - 1;
                j = end;

                // backward shrink
                while (j >= i) {
                    if (S.charAt(j) == T.charAt(k)) {
                        k--;
                        if (k < 0) break;
                    }
                    j--;
                }

                int newStart = j;
                int len = end - newStart + 1;

                if (len < minLen) {
                    minLen = len;
                    startIndex = newStart;
                }
            }
        }

        return startIndex == -1 ? "" : S.substring(startIndex, startIndex + minLen);
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

S = "abcdebdde"
T = "bde"

Start at index 1:
Forward match → reaches index 4
Backward shrink → window = "bcde"

Later window "bdde" also valid but starts later

Answer = "bcde"

---

## Example 2:

S = "hello"
T = "eo"

Forward match from index 1 → matches
Backward shrink → "ello"

Answer = "ello"
