# Intuition

We need to find all substrings of `str` (length `m = ptn.length`) that are an anagram of `ptn`.
Two substrings are anagrams if they have the **same frequency of characters**.

Brute force:

* For each substring of length `m`, check if it’s an anagram of `ptn`.
* That’s `O((n-m+1) * m)` → too slow for `n = 10^4`.

Optimized:

* Use **sliding window with frequency arrays**:

  * Maintain frequency of `ptn` (`freqP`).
  * Maintain frequency of current window in `str` (`freqWin`).
  * Slide window over `str`, updating `freqWin` in **O(1)** per move.
  * Compare `freqP` and `freqWin` in **O(26)** (constant).

So final complexity: **O(n \* 26) \~ O(n)**, which is efficient.

---

# Approach

1. Build frequency count of `ptn` → `freqP[26]`.
2. Initialize window of first `m` chars in `str` → `freqWin[26]`.
3. If `freqP == freqWin` → add index `0`.
4. Slide window from i=1 to n-m:

   * Remove char at left end.
   * Add char at right end.
   * Compare arrays. If equal → add index.
5. Return indices.

---
# Complexity

* Building frequency arrays: `O(m + n)`.
* Sliding window with comparison: `O(n * 26)` (since alphabet size fixed).
* **Time = O(n)**, **Space = O(26) \~ O(1)**.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<Integer> findAnagramsIndices(String str, String ptn, int n, int m) {
        ArrayList<Integer> result = new ArrayList<>();
        if (m > n) return result;

        int[] freqP = new int[26];
        int[] freqWin = new int[26];

        // Frequency of pattern
        for (int i = 0; i < m; i++) {
            freqP[ptn.charAt(i) - 'A']++;
            freqWin[str.charAt(i) - 'A']++;
        }

        // First window
        if (Arrays.equals(freqP, freqWin)) {
            result.add(0);
        }

        // Slide window
        for (int i = m; i < n; i++) {
            freqWin[str.charAt(i - m) - 'A']--;  // remove left char
            freqWin[str.charAt(i) - 'A']++;      // add right char
            
            if (Arrays.equals(freqP, freqWin)) {
                result.add(i - m + 1);
            }
        }

        return result;
    }
}
```

---

## **Walkthrough Example**

Input:

```
STR = CBAEBABACD
PTR = ABC
```

* `freqP` → {A=1, B=1, C=1}
* Window \[0,2] = "CBA" → freq matches → index 0
* Window \[1,3] = "BAE" → no match
* Window \[6,8] = "BAC" → freq matches → index 6
  Output: `[0, 6]`

---
