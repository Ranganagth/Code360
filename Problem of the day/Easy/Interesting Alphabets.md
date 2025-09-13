# Intuition

We need to print a character pattern of size `N`.

* The last character in the pattern is always the `N`th capital English letter (A → 1, B → 2, …, Z → 26).
* Each row `i` starts from a decreasing character and goes up to that last letter.

For example:
If `N = 5`, the last character is **E**.

* Row 1 starts with **E** → `E`
* Row 2 starts with **D** → `DE`
* Row 3 starts with **C** → `CDE`
* Row 4 starts with **B** → `BCDE`
* Row 5 starts with **A** → `ABCDE`

So, row `i` always starts from `(N - i + 1)`th letter and ends at the `N`th letter.

---

# Approach

1. Find the last character: `(char)('A' + N - 1)`.
2. For each row `i` from `1` to `N`:

   * Start character = `(char)('A' + N - i)`
   * Append all characters from `start` to `last`.
3. Store each row as a list of characters inside the result list.

---

# Complexity

* **Time Complexity**: `O(N^2)` → Each row may contain up to `N` characters.
* **Space Complexity**: `O(N^2)` → To store the result pattern.

---

# Code

```java
import java.util.*;

public class Solution {

    public static ArrayList<ArrayList<Character>> interestingPattern(int N) {
        ArrayList<ArrayList<Character>> result = new ArrayList<>();

        // Last character to print
        char lastChar = (char) ('A' + N - 1);

        for (int i = 1; i <= N; i++) {
            ArrayList<Character> row = new ArrayList<>();

            // Starting character for this row
            char startChar = (char) ('A' + N - i);

            // Add characters from startChar up to lastChar
            for (char c = startChar; c <= lastChar; c++) {
                row.add(c);
            }

            result.add(row);
        }

        return result;
    }
}
```

---

## Example Walkthrough

Input: `N = 4`

* Last character = `'D'`
* Row 1: Start `'D'` → `D`
* Row 2: Start `'C'` → `CD`
* Row 3: Start `'B'` → `BCD`
* Row 4: Start `'A'` → `ABCD`

Output:

```
D
CD
BCD
ABCD
```

---
