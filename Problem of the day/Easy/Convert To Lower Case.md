# Intuition

Uppercase and lowercase English letters differ by a fixed ASCII offset.
If a character lies between `'A'` and `'Z'`, converting it to lowercase means shifting it to the corresponding `'a'`–`'z'` range.
Characters already in lowercase must remain unchanged.

---

# Approach

1. Convert the string into a character array.
2. Traverse each character:

   * If the character is between `'A'` and `'Z'`, convert it to lowercase by adding the ASCII difference (`'a' - 'A'`).
   * Otherwise, leave it unchanged.
3. Build and return the resulting string.

---

# Complexity

* **Time Complexity:** **O(n)**, where `n` is the length of the string
* **Space Complexity:** **O(n)**, for the character array

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String toLowerCase(String str) {

        char[] chars = str.toCharArray();

        for (int i = 0; i < chars.length; i++) {
            if (chars[i] >= 'A' && chars[i] <= 'Z') {
                chars[i] = (char)(chars[i] + ('a' - 'A'));
            }
        }

        return new String(chars);
    }
};

```

---

# Example Walkthrough

**Input**

```
str = "AbcdEfgh"
```

**Step-by-step:**

* `'A'` → `'a'`
* `'b'` → `'b'` (already lowercase)
* `'c'` → `'c'`
* `'d'` → `'d'`
* `'E'` → `'e'`
* `'f'` → `'f'`
* `'g'` → `'g'`
* `'h'` → `'h'`

**Output**

```
"abcdefgh"
```

---

## Key Insight

Lowercase conversion is a direct ASCII transformation applied only to uppercase characters.
