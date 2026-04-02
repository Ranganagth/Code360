# Intuition

Each digit maps to multiple characters (like old phone keypad).
This is a **cartesian product / backtracking problem**.

Example:
$$"23" → ["abc"] × ["def"]$$

Generate all combinations by picking one character per digit.

---

# Approach

1. Create digit → letters mapping
2. Use **backtracking (DFS)**:

   * for each digit → try all mapped characters
   * build string step by step
3. Add complete strings to result
4. Result is naturally lexicographically sorted (due to mapping order)

---

# Complexity

* **Time complexity:**
$$O(4^N)$$
(max 4 letters per digit)

* **Space complexity:**
$$O(N)$$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    static String[] map = {
        "",     // 0
        "",     // 1
        "abc",  // 2
        "def",  // 3
        "ghi",  // 4
        "jkl",  // 5
        "mno",  // 6
        "pqrs", // 7
        "tuv",  // 8
        "wxyz"  // 9
    };

    public static List<String> findPossibleWords(String s) {

        List<String> result = new ArrayList<>();

        if (s == null || s.length() == 0) return result;

        backtrack(0, s, new StringBuilder(), result);

        return result;
    }

    private static void backtrack(int index, String s,
                                  StringBuilder curr,
                                  List<String> result) {

        if (index == s.length()) {
            result.add(curr.toString());
            return;
        }

        String letters = map[s.charAt(index) - '0'];

        for (char ch : letters.toCharArray()) {
            curr.append(ch);
            backtrack(index + 1, s, curr, result);
            curr.deleteCharAt(curr.length() - 1);
        }
    }
};

```

---

# Example walkthrough

Input:
$$"34"$$

Mapping:
$$3 → def$$


Output:
$$dg dh di eg eh ei fg fh fi$$

---

# Key Insight

- Backtracking generates all combinations in lexicographic order naturally