# Intuition

We want to divide a given string `WORD` into exactly `N` equal parts.
For that:

* The length of the string `|WORD|` must be divisible by `N`.
* If it is divisible, each substring will have a length `|WORD| / N`.
* We can then iterate through the string in chunks of that length and collect substrings.
* If not divisible, the result is impossible, so we return an empty list.

---

# Approach

1. Compute `len = word.length()`.
2. If `len % n != 0`, return an empty list (since equal division is impossible).
3. Otherwise, compute `partLen = len / n`.
4. Iterate over the string in steps of `partLen`:
   * Take substring from index `i` to `i + partLen`.
   * Add it to the result list.
5. Return the result list.

---

# Complexity

* **Time Complexity**:
  Each character is visited once, so **O(|WORD|)**.

* **Space Complexity**:
  The result stores all parts, so **O(|WORD|)**.

---

# Code

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
    public static ArrayList<String> divideString(String word, int n) {
        ArrayList<String> result = new ArrayList<>();
        int len = word.length();

        // Check if divisible
        if (len % n != 0) {
            return result; // empty list
        }

        int partLen = len / n;
        for (int i = 0; i < len; i += partLen) {
            result.add(word.substring(i, i + partLen));
        }

        return result;
    }
}
```

---

