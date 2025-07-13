### Problem Summary

You're given a **postfix expression** where:
* Operands are numbers (can be multiple digits).
* Operators are basic arithmetic: `+`, `-`, `*`, `/`.
* Tokens (numbers and operators) are separated by spaces.

Your task is to **evaluate** the postfix expression **modulo (10⁹ + 7)**. You also need to handle **modular division** properly.

---

# Intuition

In **postfix notation**, we evaluate expressions using a **stack**:

1. When you see a number → **push it** to the stack.
2. When you see an operator → **pop** the top two operands, perform the operation, and **push** the result back.

Example:
Postfix = `2 3 1 * + 9 -`
Evaluation: `2 + (3 * 1) - 9 = -4`

---

# Approach

1. **Split** the input string by space.
2. Use a **stack** to evaluate the expression.
3. For each token:
   * If it’s a number → push to stack.
   * If it’s an operator:
     * Pop two elements.
     * Apply the operation using **modular arithmetic**.
     * Push result back.
4. Finally, return the top of the stack.

**Modular division** uses **modular inverse**:

For `a / b mod M`, we use:
→ `a * modInverse(b, M) % M`
(using Fermat’s Little Theorem: `b^(M-2) % M` if M is prime)

---

# Complexity Analysis

* **Time**: O(N), where N is the number of tokens.
* **Space**: O(N), for the stack.

---

# Code

```java
import java.util.*;

public class Solution {
    static final int MOD = 1_000_000_007;

    public static int evaluatePostfix(String[] exp) {
        Stack<Long> stack = new Stack<>();

        for (String token : exp) {
            if (isOperator(token)) {
                long b = stack.pop();
                long a = stack.pop();
                long result = applyOperator(a, b, token);
                stack.push(result);
            } else {
                stack.push(Long.parseLong(token));
            }
        }

        return stack.peek().intValue(); // Return final result
    }

    private static boolean isOperator(String token) {
        return token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/");
    }

    private static long applyOperator(long a, long b, String op) {
        switch (op) {
            case "+":
                return (a + b) % MOD;
            case "-":
                return ((a - b) % MOD + MOD) % MOD;
            case "*":
                return (a * b) % MOD;
            case "/":
                return (a * modInverse(b, MOD)) % MOD;
        }
        return 0;
    }

    // Modular inverse using Fermat's little theorem
    private static long modInverse(long b, int mod) {
        return power(b, mod - 2, mod);
    }

    private static long power(long x, long y, int mod) {
        long res = 1;
        x = x % mod;
        while (y > 0) {
            if ((y & 1) == 1)
                res = (res * x) % mod;
            x = (x * x) % mod;
            y >>= 1;
        }
        return res;
    }
}
```

---

### **Example Walkthrough**

#### Input:

```
exp = ["2", "3", "1", "*", "+", "9", "-"]
```

#### Step-by-step:

* Stack: [2]
* Stack: [2, 3]
* Stack: [2, 3, 1]
* Apply "*": → 3 * 1 = 3 → Stack: [2, 3]
* Apply "+": → 2 + 3 = 5 → Stack: [5]
* Push 9 → Stack: [5, 9]
* Apply "-": → 5 - 9 = -4 → Stack: [-4]

**Final Answer**: `-4 % MOD = 1000000003`

---

### **Summary**

* Handles **large integers** with modular arithmetic.
* Correctly computes **modular division** with Fermat’s theorem.
* Modular negative handling: `((a - b) % MOD + MOD) % MOD` ensures non-negative results.