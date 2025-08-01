# Intuition

We need to generate all binary strings of length `N` such that:
* Each string consists of only `0` and `1`
* No two `1`s are adjacent
* Strings must be in **lexicographically increasing order**

---

# Approach

We use **backtracking / recursion**:
* Start with an empty string and at each step, decide whether to add `'0'` or `'1'`.
* **We can always add `'0'`** regardless of the last character.
* **We can add `'1'` only if the previous character was `'0'`** (or it's the first character).
* Recursively build the string until its length becomes `N`, then add it to the result.

---

# Complexity

* **Time complexity:** `O(2^N)` in the worst case (when many strings satisfy the condition)
* **Space complexity:** `O(N)` recursion depth + space to store results

---

# Code

```java
import java.util.*;

public class Solution {
    public static List<String> generateString(int N) {
        List<String> result = new ArrayList<>();
        generate(N, "", result, '0'); // Start assuming previous char is '0'
        return result;
    }

    private static void generate(int N, String current, List<String> result, char prevChar) {
        if (current.length() == N) {
            result.add(current);
            return;
        }

        // Always safe to add '0'
        generate(N, current + "0", result, '0');

        // Add '1' only if previous was '0'
        if (prevChar == '0') {
            generate(N, current + "1", result, '1');
        }
    }
}
```

---

### **Example Walkthrough**

For `N = 3`, the valid binary strings (no two 1s together) are:

```
000
001
010
100
101
```

All strings are printed in **lexicographic order**.

---
