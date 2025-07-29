# Intuition

To reverse substrings within matching parentheses from the innermost level outward, a **stack** is ideal. It helps in tracking open parentheses and manipulating nested segments by treating each as an isolated "layer" of operation.

---

# Approach

* Use a **stack** to process characters one by one.
* When encountering a **')'**, start popping characters from the stack until a **'('** is found. Reverse the collected characters and push them back to the stack.
* At the end, the stack contains the fully processed characters in order.
* Join and return the final result.

This works well because innermost substrings are processed first—exactly what a stack's LIFO property provides.

---

# Complexity

* Time complexity:
  $O(n)$ - Each character is pushed and popped at most once.

* Space complexity:
  $O(n)$ - The stack can store up to all characters in the string.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String reverseStringsInParentheses(String s, int n) {
        Stack<Character> stack = new Stack<>();

        for (char ch : s.toCharArray()) {
            if (ch == ')') {
                // Pop characters until matching '('
                StringBuilder temp = new StringBuilder();
                while (!stack.isEmpty() && stack.peek() != '(') {
                    temp.append(stack.pop());
                }

                // Discard the '('
                if (!stack.isEmpty()) stack.pop();

                // Push reversed characters back to stack
                for (char c : temp.toString().toCharArray()) {
                    stack.push(c);
                }
            } else {
                stack.push(ch);
            }
        }

        // Construct final result from the stack
        StringBuilder result = new StringBuilder();
        while (!stack.isEmpty()) {
            result.append(stack.pop());
        }

        return result.reverse().toString();  // Reverse because stack gives chars in reverse order
    }

    // For testing multiple test cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = Integer.parseInt(sc.nextLine());
        while (T-- > 0) {
            int N = Integer.parseInt(sc.nextLine());
            String S = sc.nextLine();
            System.out.println(reverseStringsInParentheses(S, N));
        }
    }
}
```

---

