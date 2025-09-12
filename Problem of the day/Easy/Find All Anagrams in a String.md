# Intuition

* We need to check every substring of length `m` in `str` and see if it is an **anagram** of `ptr`.
* Brute force (checking each substring separately by sorting or comparing counts) would be too slow (`O(n*m)`).
* Instead, use a **sliding window + character frequency**:

  * Count the frequency of characters in `ptr`.
  * Maintain a window of size `m` in `str` with its frequency count.
  * If the two frequency arrays match → it’s an anagram → store the starting index.
  * Slide the window forward: remove the frequency of the leftmost char, add the new rightmost char.

---

# Approach

1. Initialize:

   * Array `freqPTR[26]` for counts in `ptr`.
   * Array `freqWIN[26]` for the current window of `str`.
2. Fill both arrays for the first `m` characters.
3. Compare `freqPTR` and `freqWIN`. If equal → store index `0`.
4. Slide the window from `i = m` to `n-1`:

   * Remove the left char (`str[i-m]`).
   * Add the new char (`str[i]`).
   * Compare arrays.
   * If equal → store index `(i-m+1)`.
5. Return the list of indices.

---

# Complexity

* **Time:** O(N \* 26) = O(N), since each window move updates in O(1) and comparisons are O(26).
* **Space:** O(26) = O(1).

---

# Code

```java
import java.util.ArrayList;
import java.util.Arrays;

public class Solution {
    public static ArrayList<Integer> findAnagramsIndices(String str, int n, String ptr, int m) {
        ArrayList<Integer> result = new ArrayList<>();

        if (m > n) return result;

        int[] freqPTR = new int[26];
        int[] freqWIN = new int[26];

        // Frequency of PTR
        for (int i = 0; i < m; i++) {
            freqPTR[ptr.charAt(i) - 'A']++;
            freqWIN[str.charAt(i) - 'A']++;
        }

        // First window check
        if (Arrays.equals(freqPTR, freqWIN)) {
            result.add(0);
        }

        // Slide window
        for (int i = m; i < n; i++) {
            // Remove leftmost char
            freqWIN[str.charAt(i - m) - 'A']--;
            // Add new char
            freqWIN[str.charAt(i) - 'A']++;

            if (Arrays.equals(freqPTR, freqWIN)) {
                result.add(i - m + 1);
            }
        }

        return result;
    }
}
```

---

## Example Walkthrough

Input:

```
STR = "CBAEBABACD", N=10
PTR = "ABC", M=3
```

* `freqPTR = {A:1, B:1, C:1}`
* Initial window `"CBA"` → freqWIN = {A:1, B:1, C:1} → match → add `0`.
* Slide:

  * Window `"BAE"` → mismatch.
  * `"AEB"` → mismatch.
  * `"EBA"` → mismatch.
  * `"BAB"` → mismatch.
  * `"ABA"` → mismatch.
  * `"BAC"` → match → add `6`.
  * `"ACD"` → mismatch.

Output = `[0, 6]`.

---
