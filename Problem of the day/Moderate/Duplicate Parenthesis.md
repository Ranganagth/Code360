> (Partially Accepted - 10/11 Test cases)
# Intuition

To detect duplicate parentheses, we observe that if between a pair of parentheses there is no operator or valid expression (like just another closing parenthesis), it's redundant. So, if we encounter a closing parenthesis and the stack's top is an opening parenthesis or there are no meaningful symbols in between, then it's a duplicate.

---

# Approach

* Use a **stack** to track characters of the expression.

* Traverse the expression one character at a time:
  * If the current character is **not a closing parenthesis `)`**, push it onto the stack.
  * If the current character is a **closing parenthesis `)`**:
    * Check the top of the stack:
      * If the first popped character is `'('`, it's a **duplicate** — return `true`.
      * Otherwise, keep popping until `'('` is found.
	  
* If we finish traversing the string and don’t find any duplicates, return `false`.

---

# Complexity

* **Time complexity:**
  $O(n)$ per expression (where *n* is the length of the expression), since each character is pushed and popped from the stack once.

* **Space complexity:**
  $O(n)$ - In the worst case, the stack may hold all characters.

---

# Code

```java
import java.util.*;

public class Solution {

    public static boolean duplicateParanthesis(String expr) {
        Stack<Character> stack = new Stack<>();

        for (char ch : expr.toCharArray()) {
            if (ch == ')') {
                int count = 0;
                while (!stack.isEmpty() && stack.peek() != '(') {
                    stack.pop();
                    count++;
                }
                if (!stack.isEmpty()) {
                    stack.pop(); // remove the opening '('
                }

                // if count is 0, it means there were no characters inside the brackets
                // i.e., duplicate like ()
                if (count == 0) {
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

---

### Example 2

**Input:** `((a+b)+c)`

* No point where closing bracket directly follows an opening bracket.
* All expressions are meaningful → return `false`

**Output:** `NO`

---

# Alternate Solution 

```java
import java.util.* ;
import java.io.*; 

public class Solution {

	public static boolean duplicateParanthesis(String expr) {
		Stack<Character> stack = new Stack<>();

		for (char ch : expr.toCharArray()) {
			if (ch == ')') {
				int elementsInside = 0;

				while (!stack.isEmpty && stack.peek() != '(') {
					stack.pop();
					elementsInside++;
				}

				if (stack.isEmpty()) {
					continue;
				}

				int numPopped = 0;
				while (!stack.isEmpty() && stack.peek() != '(') {
					char
				}
			}
		}
	}

}

```