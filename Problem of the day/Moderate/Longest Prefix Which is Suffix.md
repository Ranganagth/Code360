# Intuition

We want the **longest proper prefix** which is also a **suffix**. A *proper* prefix/suffix means it **cannot be the entire string**.

The **LPS array** for a string stores, at each index `i`, the length of the longest proper prefix which is also a suffix for the substring `str[0..i]`.

Thus, the **last value** of the LPS array gives us the required length.

---

# Approach (Using KMP LPS Array)

1. Initialize an LPS array of length `n`.
2. Use two pointers: `len` to keep track of current matching prefix and suffix, and `i` to traverse the string.
3. While traversing:

   * If `str[i] == str[len]`, we have a match, so we increment `len` and store it in `lps[i]`.
   * Else if there’s a mismatch and `len != 0`, we fall back to the `lps[len - 1]`.
   * If `len == 0`, just move ahead.
4. At the end, `lps[n - 1]` gives us the required length.

---
# Complexity

* **Time Complexity:** O(n), where n is the length of the string
* **Space Complexity:** O(n) for the LPS array

---

# Code

```java
public class Solution {
    public static String longestPrefixSuffix(String str) {
        int n = str.length();
        int[] lps = new int[n];

        int len = 0;
        int i = 1;

        // Build LPS array
        while (i < n) {
            if (str.charAt(i) == str.charAt(len)) {
                len++;
                lps[i] = len;
                i++;
            } else {
                if (len != 0) {
                    len = lps[len - 1]; // fallback
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }

        int lpsLength = lps[n - 1];

        // If no such prefix-suffix found
        if (lpsLength == 0) {
            return "";
        }

        // Return the prefix substring
        return str.substring(0, lpsLength);
    }
}
```

---
### Example Walkthrough

**Input: `"ababcdabab"`**

* LPS array becomes: `[0, 0, 1, 2, 0, 0, 1, 2, 3, 4]`
* The last value `lps[9] = 4`, so the result is `str.substring(0, 4) = "abab"`

---

### Sample Input / Output

**Sample Input 1:**

```text
aaaaabaa
```

* LPS: `[0, 1, 2, 3, 4, 5, 0, 1]`
* Output: `"aa"`

**Sample Input 2:**

```text
aab
```

* LPS: `[0, 1, 0]`
* Output: `""` (Console prints `-1`)

---