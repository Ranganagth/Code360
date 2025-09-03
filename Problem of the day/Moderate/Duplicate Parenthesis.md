# Intuition

* Duplicate parentheses occur when **two parentheses wrap around nothing but a single valid subexpression already enclosed by parentheses**.
* For example:

  * `"((c+d))"` → inner `(c+d)` is valid, but the outer `(...)` adds nothing new → **duplicate**.
  * `"((a+b) + c)"` → both sets are needed (outer wraps more than just the inner expression) → **not duplicate**.

The key observation:

* When we encounter a `')'`, if the character just inside it is `'('` → **empty parentheses** → duplicate.
* Or, if between `'('` and `')'` there is **only one subexpression without operators/extra tokens**, it means redundancy.

---

# Approach

1. Use a **stack**.
2. Traverse the expression character by character:

   * If character is anything other than `')'`, push onto stack.
   * If character is `')'`:

     * Pop until `'('`.
     * Count how many characters were inside.
     * If **count < 1**, it means `()` or `((expr))` type → duplicate → return `true`.
3. If traversal finishes without detecting duplicates → return `false`.

---

# Complexity

* Each character is pushed and popped at most once.
* **Time complexity**: O(|expr|).
* **Space complexity**: O(|expr|) (stack).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    public static boolean duplicateParanthesis(String expr) {
        Stack<Character> stack = new Stack<>();

        for (char ch : expr.toCharArray()) {
            if (ch == ')') {
                // Check for content between parentheses
                int count = 0;
                while (!stack.isEmpty() && stack.peek() != '(') {
                    stack.pop();
                    count++;
                }

                // Pop the opening bracket '('
                if (!stack.isEmpty()) {
                    stack.pop();
                }

                // If there were less than 1 element inside, it's duplicate
                if (count < 1) {
                    return true;
                }
            } else {
                stack.push(ch);
            }
        }
        return false;
    }
}
```

---

# Example and Walkthrough

### Example 1

**Input:** `(a+b)+((c+d))`

* First set `(a+b)` is fine.
* Then `((c+d))`:

  * We encounter `)` → top of the stack is `d`, pop till `(` → count = 3 (`d`, `+`, `c`)
  * Pop `(` → continue
  * Next `)` → top is `(` → **duplicate**, return `true`

**Output:** `YES`

### Example 2

**Input:** `((a+b)+c)`

* No point where closing bracket directly follows an opening bracket.
* All expressions are meaningful → return `false`

**Output:** `NO`

### Example 3

**Input:** `(a+b)+((c+d))`

- Push `(a+b)+((c+d`.
- When reaching last `)`: pop until `'('` → popped `c+d` (count = 3).
- Another `)` comes: pops `(c+d)` as one unit → **count = 0** → duplicate found → return `true`.

**Output:** `YES`.

---