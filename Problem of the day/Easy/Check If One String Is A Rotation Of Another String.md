# Intuition

`String.contains()` may internally degrade toward slower substring matching for large inputs (up to (10^5)), especially across multiple test cases.
To guarantee strict (O(N)) time, use **KMP (Knuth-Morris-Pratt)** string matching.

If `Q` is a cyclic rotation of `P`, then `Q` must appear as a substring inside:

```
P + P
```

KMP ensures linear-time substring search.

---

# Approach

1. If lengths differ → return `0`.
2. Construct:

   ```
   text = P + P
   pattern = Q
   ```
3. Build LPS (Longest Prefix Suffix) array for pattern.
4. Run KMP search to check if `pattern` exists inside `text`.
5. Return `1` if found, otherwise `0`.

---

# Complexity

* **Time complexity:** (O(N))
* **Space complexity:** (O(N))

---

# Code

```java
public class Solution {

    public static int isCyclicRotation(String p, String q) {

        if (p.length() != q.length())
            return 0;

        String text = p + p;

        return kmpSearch(text, q) ? 1 : 0;
    }

    // KMP Search
    private static boolean kmpSearch(String text, String pattern) {

        int n = text.length();
        int m = pattern.length();

        int[] lps = buildLPS(pattern);

        int i = 0, j = 0;

        while (i < n) {

            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;

                if (j == m) return true;
            }
            else if (j > 0) {
                j = lps[j - 1];
            }
            else {
                i++;
            }
        }
        return false;
    }

    // Build LPS Array
    private static int[] buildLPS(String pattern) {

        int m = pattern.length();
        int[] lps = new int[m];

        int len = 0;
        int i = 1;

        while (i < m) {

            if (pattern.charAt(i) == pattern.charAt(len)) {
                lps[i++] = ++len;
            }
            else if (len > 0) {
                len = lps[len - 1];
            }
            else {
                lps[i++] = 0;
            }
        }

        return lps;
    }
};

```

---

# Example walkthrough with explanation

Example:

```
P = abac
Q = baca
```

Construct:

```
text = abacabac
pattern = baca
```

KMP efficiently scans once and finds `"baca"`.

Return:

```
1
```
