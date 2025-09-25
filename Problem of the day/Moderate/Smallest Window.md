# Intuition

We want the smallest substring in `S` that contains **all characters of `X` (with multiplicity)**.
This is a **sliding window problem**:

* Expand the right pointer until the window contains all characters of `X`.
* Then shrink the left pointer as much as possible while still keeping all required characters.
* Update the minimum window whenever conditions are satisfied.
* Continue until the right pointer finishes scanning `S`.

---

# Approach

1. **Frequency map of X**:
   Count how many times each character appears in `X`. Let’s call this `need`.

2. **Sliding window** with two pointers `left` and `right`:

   * Move `right` to expand the window. Track characters in `windowCount`.
   * If a character count in `windowCount` matches that in `need`, we consider that character as satisfied.

3. **Check validity**:
   If all characters from `X` are satisfied in the current window, try shrinking from `left`:

   * While shrinking, keep the substring valid.
   * Update the minimum window length if the current window is smaller.

4. **Result**:
   If no valid window exists, return `""`. Otherwise return the best window.

---

# Complexity

* **Time**: O(|S| + |X|)

  * Each character is processed at most twice (expand + contract).
* **Space**: O(Alphabet size)

  * Frequency maps for `need` and `windowCount`.

---

# Code

```java
import java.util.*;

public class Solution {
    public static String smallestWindow(String s, String x) {
        if (s.length() < x.length()) return "";

        // Frequency map of required characters
        Map<Character, Integer> need = new HashMap<>();
        for (char c : x.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        // Sliding window
        Map<Character, Integer> window = new HashMap<>();
        int have = 0, needCount = need.size();
        int left = 0, minLen = Integer.MAX_VALUE, start = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            window.put(c, window.getOrDefault(c, 0) + 1);

            if (need.containsKey(c) && window.get(c).intValue() == need.get(c).intValue()) {
                have++;
            }

            // Shrink while valid
            while (have == needCount) {
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }

                char leftChar = s.charAt(left);
                window.put(leftChar, window.get(leftChar) - 1);
                if (need.containsKey(leftChar) && window.get(leftChar) < need.get(leftChar)) {
                    have--;
                }
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
    }
}
```

---

# Example Walkthrough

S = `"kmmdnj"`, X = `"mj"`

* `need = {m:1, j:1}`
* Expand:

  * `k` → not useful
  * `m` → now window has 1 `m`
  * `m` again → window has 2 `m`
  * `d` → still no `j`
  * `n` → still no `j`
  * `j` → now window is `"kmmdnj"`, valid.
* Shrink from left:

  * Remove `k` → `"mmdnj"`, still valid.
  * Remove first `m` → `"mdnj"`, still valid.
  * Try removing `d` → `"mnj"`, still valid.
  * Try removing `n` → `"mj"`, still valid and smaller.
* Minimum found = `"mj"`.

---
