# Intuition

We’re simulating a **score record** based on operations:

* **Integers** are added as new scores.
* `"+"` adds the sum of the last 2 scores.
* `"D"` adds double the last score.
* `"C"` removes the last score.

A **stack** is a perfect data structure for this. We’ll:

* Push scores when adding.
* Pop the last score when canceling.
* Peek at previous scores for "+" and "D".

---

# Approach

1. Initialize a stack of integers.
2. Loop over each operation in `matchResult`:
   * If it's a number → push it.
   * If `"+"` → sum of last 2 scores → push.
   * If `"D"` → double of last score → push.
   * If `"C"` → remove last score → pop.
3. After processing all operations, sum up the stack values.

---
# Complexity

* **Time:** `O(n)` — each operation is processed once
* **Space:** `O(n)` — for the stack storing scores

---
# Code

```java
import java.util.*;

public class Solution {
    public static int calculateScore(ArrayList<String> matchResult, int n) {
        Stack<Integer> stack = new Stack<>();

        for (String op : matchResult) {
            if (op.equals("+")) {
                int last = stack.pop();
                int secondLast = stack.peek();
                int newScore = last + secondLast;
                stack.push(last); // put last back
                stack.push(newScore);
            } else if (op.equals("D")) {
                stack.push(2 * stack.peek());
            } else if (op.equals("C")) {
                stack.pop();
            } else {
                stack.push(Integer.parseInt(op));
            }
        }

        int sum = 0;
        for (int score : stack) {
            sum += score;
        }
        return sum;
    }
}
```

---

### **Example Walkthrough**

#### Sample Input:

```
MATCHRESULT = ["2", "3", "+", "D", "C"]
```

#### Stack trace:

1. `"2"` → `[2]`
2. `"3"` → `[2, 3]`
3. `"+"` → sum of 2+3 = 5 → `[2, 3, 5]`
4. `"D"` → 2×5 = 10 → `[2, 3, 5, 10]`
5. `"C"` → remove 10 → `[2, 3, 5]`

Final score = `2 + 3 + 5 = 10`

---
