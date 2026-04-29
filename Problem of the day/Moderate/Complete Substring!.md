# Intuition

Find the **minimum window substring** in `S1` that contains all characters of `S2` (with frequency).

Use **sliding window + frequency counting**:

* Expand window to satisfy requirement
* Shrink window to minimize length

---

# Approach

1. Create frequency array `need[26]` for `S2`
2. Maintain:

   * `required = length of S2`
   * `left = 0`
   * `minLen`, `start`
3. Traverse with `right`:

   * Decrease `need[s1[right]]`
   * If character was needed → `required--`
4. When `required == 0`:

   * Try shrinking from left
   * Update minimum window
   * If removing breaks condition → stop shrinking
5. Return substring if found, else empty string

---

# Complexity

* **Time complexity:**
  $$O(n)$$

* **Space complexity:**
  $$O(1)$$ (fixed 26 size)

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution{
    public static String findSubString(String s1, String s2){

        int[] need = new int[26];

        for (char c : s2.toCharArray()) {
            need[c - 'A']++;
        }

        int required = s2.length();
        int left = 0, start = 0, minLen = Integer.MAX_VALUE;

        for (int right = 0; right < s1.length(); right++) {

            char rc = s1.charAt(right);

            if (need[rc - 'A'] > 0) {
                required--;
            }
            need[rc - 'A']--;

            while (required == 0) {

                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }

                char lc = s1.charAt(left);
                need[lc - 'A']++;

                if (need[lc - 'A'] > 0) {
                    required++;
                }

                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s1.substring(start, start + minLen);
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

S1 = "ACBBBCA"
S2 = "ABC"

Process:

* Expand window until contains A, B, C
* Window: "ACB" → valid
* Try shrink → minimal stays "ACB"

Answer = "ACB"

---

## Example 2:

S1 = "ABBBB"
S2 = "ABC"

No 'C' in S1

Answer = ""
