### Problem Summary

You’re given a string `STR` containing:
* Opening `(` and closing `)` parentheses
* Lowercase English letters

Your goal is to **remove the minimum number of parentheses** to make the string **valid**, and return **all possible unique valid strings** formed by such removals.

A **valid** string:
* Has matching open-close parentheses
* Can contain letters
* Can be an empty string

---

### Example:

**Input**: `()())`
**Output**: `["()()", "(())"]`
Explanation:

* You can remove one `)` to get either `"()()"` or `"(())"`.
* Both are valid and created with **minimum** removals (just 1).

---

# Intuition

This is a classic problem of generating all valid strings by removing invalid parentheses with **minimum removals**.

Key challenges:

* **Avoid duplicates**
* **Stop at minimum number of removals**
* **Generate all possible combinations**

---

# Approach (BFS – Breadth-First Search):

1. **Start with the input string**.
2. **Use BFS** to explore strings formed by removing one parenthesis at a time.
3. For each new string:
   * Check if it's valid
   * If valid and we haven't found any valid strings yet, add to result
   * If we already found valid strings in this level, **don’t go deeper**, as we want only minimum removal results
4. Use a **Set** to avoid revisiting the same strings.

---
# Complexity Analysis

Let `n = str.length()`:

* **Time Complexity**:
  * In the worst case, we may generate `2^n` strings (every subset by removing parentheses)
  * But we **stop** at the first level that yields valid strings → this **reduces depth significantly**

* **Space Complexity**:
  * $O(2^n)$ for `visited` and `queue`, but again, it’s bounded due to early stopping

---
# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<String> minRemovaltoMakeStringValid(String str) {
        Set<String> result = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();

        queue.add(str);
        visited.add(str);
        boolean found = false;

        while (!queue.isEmpty()) {
            String current = queue.poll();

            if (isValid(current)) {
                result.add(current);
                found = true; // Found at least one valid at this level
            }

            if (found) continue; // Don't generate next level if valid found

            for (int i = 0; i < current.length(); i++) {
                char ch = current.charAt(i);
                if (ch != '(' && ch != ')') continue;

                String next = current.substring(0, i) + current.substring(i + 1);
                if (!visited.contains(next)) {
                    queue.add(next);
                    visited.add(next);
                }
            }
        }

        return new ArrayList<>(result);
    }

    private static boolean isValid(String s) {
        int count = 0;
        for (char ch : s.toCharArray()) {
            if (ch == '(') count++;
            else if (ch == ')') {
                if (count == 0) return false;
                count--;
            }
        }
        return count == 0;
    }
}
```

---

### **Example Walkthrough**

Input: `"()())"`
**Initial Queue**: `["()())"]`

Level 1:

* Remove one character each time:

  * `"()))"`, `"(()))"`, `"()()"`, `"()())"` (duplicates ignored)
* Valid strings: `"(())"`, `"()()"`

**Found valid**, stop exploring deeper levels.

Output: `["(())", "()()"]`

---

### **Summary**

* This is a **BFS + pruning** problem
* Guarantees **minimum number of removals**
* Ensures **all unique valid strings**
* Could be adapted to DFS, but BFS guarantees minimal removals due to level-order traversal