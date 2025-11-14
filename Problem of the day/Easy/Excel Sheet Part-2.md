# Intuition

This problem is identical to converting a **base-26 alphabetic system** (like Excel columns) into a **decimal number**.
Each character represents a digit where `'A' → 1`, `'B' → 2`, … `'Z' → 26`.
The string is effectively a number in base 26, but **without a zero digit**, so we compute the value cumulatively from left to right.

---

# Approach

1. Initialize `result = 0`.
2. For each character `c` in the string:

   * Convert it to its numeric value: `val = c - 'A' + 1`.
   * Multiply the current `result` by 26 and add `val`.
3. After processing all characters, `result` gives the corresponding Excel column number.

This mimics base conversion:
For `"AB"` → `(1 × 26¹) + (2 × 26⁰) = 28`.

---

# Complexity

* **Time complexity:** O(|STR|)
  Each character is processed once.
* **Space complexity:** O(1)
  Only a few variables are used.

---

# Code

```java
import java.util.*;

public class Solution {
    public static long titleToNumber(String str) {
        long result = 0;
        for (int i = 0; i < str.length(); i++) {
            int val = str.charAt(i) - 'A' + 1;
            result = result * 26 + val;
        }
        return result;
    }
}
```

---

# Example walkthrough with explanation

**Input:** `"AB"`

| Step | Character | Value | Calculation  | Result |
| ---- | --------- | ----- | ------------ | ------ |
| 1    | 'A'       | 1     | (0 × 26) + 1 | 1      |
| 2    | 'B'       | 2     | (1 × 26) + 2 | 28     |

**Output:** `28` 
