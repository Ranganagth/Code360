# Intuition

Instead of generating all substrings (which is O(n²) and inefficient), we can use the **sliding window + hashmap** approach:
* Use a sliding window to maintain a substring that has at most **2 unique characters**.
* As you move the end of the window, keep expanding.
* If the count of unique characters exceeds 2, move the start of the window forward until it becomes valid again.
* For each valid window, the number of substrings ending at position `end` is `(end - start + 1)`.

---

# Approach

1. Initialize a sliding window with two pointers: `start` and `end`.
2. Use a `HashMap<Character, Integer>` to track frequency of characters in the current window.
3. For each character:

   * Add it to the window.
   * If the number of unique characters exceeds 2, shrink the window from the start.
   * For each valid window, add `(end - start + 1)` to the result.

---

# Complexity Analysis

* **Time complexity:** O(n), where `n` is the length of the string.
* **Space complexity:** O(1) (since there are at most 3 characters: `a`, `b`, `c`).

---

# Code

```java
import java.util.*;

public class Solution {
    public static int totalStrings(String s) {
        int n = s.length();
        int start = 0, end = 0;
        int result = 0;
        Map<Character, Integer> map = new HashMap<>();

        while (end < n) {
            char ch = s.charAt(end);
            map.put(ch, map.getOrDefault(ch, 0) + 1);

            // Shrink window if more than 2 distinct characters
            while (map.size() > 2) {
                char startChar = s.charAt(start);
                map.put(startChar, map.get(startChar) - 1);
                if (map.get(startChar) == 0) {
                    map.remove(startChar);
                }
                start++;
            }

            // Add valid substrings ending at 'end'
            result += (end - start + 1);
            end++;
        }

        return result;
    }
}
```

---

### Example

**Input:**

```
s = "abc"
```

**Substrings with ≤ 2 unique characters:**
`a`, `b`, `c`, `ab`, `bc` → **Answer: 5**

