# Intuition

* Not all uppercase letters look the same when reflected in a mirror.
* For example, **A, H, I, M, O, T, U, V, W, X, Y** remain the same in a mirror.
* Any letter outside this set (like `B`, `E`, `Z`, etc.) cannot form a valid mirrored string.
* To be mirror-symmetric, the string must also be **palindromic** in terms of these valid mirrored characters.

---

# Approach

1. **Create a set** of mirror-symmetric characters.

2. For each character in the string:
   * Check if it belongs to the mirror set.
   * Check if it is equal to its counterpart from the end (like in a palindrome).

3. If all checks pass, return `true`; else return `false`.

---

# Complexity Analysis

* **Time:** O(n), where `n` is the length of the string.
* **Space:** O(1) (constant space for the mirror character set).

---

# Code

```java
import java.util.*;

public class Solution {
    public static Boolean isReflectionEqual(String s) {
        Set<Character> mirrorChars = new HashSet<>(Arrays.asList(
            'A', 'H', 'I', 'M', 'O', 'T', 'U', 'V', 'W', 'X', 'Y'
        ));

        int left = 0;
        int right = s.length() - 1;

        while (left <= right) {
            char lChar = s.charAt(left);
            char rChar = s.charAt(right);

            // Both must be in mirror set and equal
            if (!mirrorChars.contains(lChar) || !mirrorChars.contains(rChar) || lChar != rChar) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

---

### Example

**Input:** `"ITATI"`
**Output:** `YES`

**Input:** `"MZM"`
**Output:** `NO` (Z is not a valid mirror letter)

---
