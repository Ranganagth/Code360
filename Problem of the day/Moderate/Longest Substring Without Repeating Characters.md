# Intuition

We use the **sliding window technique** with two pointers and a HashMap to track characters we've seen and their most recent positions.

The goal is to maintain a window `[start, end]` that **always contains unique characters**:
* If we encounter a duplicate character, we **move the `start` pointer** past its last occurrence.
* At each step, update the maximum length of the current window.

---

# Approach

1. Initialize `start = 0`, `maxLength = 0`, and a `Map<Character, Integer>` to store the last seen index of characters.
2. Traverse the string with an `end` pointer.
   * If the current character exists in the map **and its index is ≥ start**, move `start` to `map.get(ch) + 1`.
   * Always update the last seen index for `ch`.
   * Update `maxLength = max(maxLength, end - start + 1)`.

---

# Complexity Analysis

* **Time:** O(n) — Each character is processed at most twice.
* **Space:** O(k) — k is the character set size (26 lowercase letters).

---

# Code

```java
import java.util.*;

public class Solution {
    public static int uniqueSubstrings(String input) {
        int n = input.length();
        int maxLength = 0;
        int start = 0;
        Map<Character, Integer> map = new HashMap<>();

        for (int end = 0; end < n; end++) {
            char ch = input.charAt(end);
            if (map.containsKey(ch) && map.get(ch) >= start) {
                // Move start just past the last occurrence of current character
                start = map.get(ch) + 1;
            }
            map.put(ch, end); // update or insert current character's latest index
            maxLength = Math.max(maxLength, end - start + 1);
        }

        return maxLength;
    }
}
```

---

### **Example Walkthrough**

#### Input: `"abcabcbb"`

* Step-by-step substrings checked: `a`, `ab`, `abc`, then `bca` (after duplicate `a`), etc.
* Longest without repetition: `"abc"` → length `3`

#### Input: `"aaaa"`

* All characters repeat → only unique substrings: `"a"` → length `1`

---
