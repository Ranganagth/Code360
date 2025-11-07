# Intuition

We need to count how many words in a given string are **palindromes**.
A **palindrome** word reads the same forwards and backwards (e.g., `"madam"`, `"level"`, `"I"`).

We must:

1. Split the string by one or more whitespaces.
2. Normalize the case (since `"Madam"` and `"madam"` are both palindromes).
3. Check each word — if it reads the same when reversed, increase count.

---

# Approach

1. **Trim and split** the string using regex:
   `"\\s+"` → handles multiple spaces or tabs.
2. **Normalize case** → convert to lowercase.
3. **Check palindrome:**
   For each word `w`, check if `w.equals(new StringBuilder(w).reverse().toString())`
4. **Count and return.**

---

## Example Walkthrough

### Example:

**Input:**
`"Madam oyo cat"`

**Steps:**

```
Words = ["Madam", "oyo", "cat"]
Normalized = ["madam", "oyo", "cat"]
Palindrome checks:
madam → palindrome
oyo   → palindrome
cat   → not palindrome
```

**Count = 2**

**Output:**
`2`

---

# Complexity Analysis

* **Time:** O(N) (we visit each character once)
* **Space:** O(N) (for storing split words and reversed string)

Works efficiently for |S| ≤ 10⁵.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int countNumberOfPalindromeWords(String s) {
        if (s == null || s.trim().isEmpty()) return 0;

        String[] words = s.trim().split("\\s+");
        int count = 0;

        for (String word : words) {
            String lower = word.toLowerCase();
            String reversed = new StringBuilder(lower).reverse().toString();
            if (lower.equals(reversed)) {
                count++;
            }
        }
        return count;
    }
};

```

---

## Example Verification

**Input:**

```
1
Nitin and I are good friends
```

**Output:**
`2`

Explanation:
“**Nitin**” and “**I**” are palindromes.

---

**Input:**

```
2
Madam taught us the level order traversal of a binary tree yesterday
We love coding ninjas
```

**Output:**

```
3
0
```

Matches expected result.

---

