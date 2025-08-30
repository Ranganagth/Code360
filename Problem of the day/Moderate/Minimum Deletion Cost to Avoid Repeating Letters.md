# Intuition

We want a string with **no two adjacent equal characters**.  
Whenever we see consecutive repeating characters, we **must delete some of them**.  

- Example: `"aaa"` with costs `[3, 2, 5]`  
  To leave only **one `a`**, we must delete others.  
  To minimize cost, we **keep the one with the maximum cost** (because deleting it would be expensive) and delete the rest.  
  So, total cost = `sum(all) - max(all)`.

Thus, for **each group of consecutive equal characters**, the answer =  
`(sum of their costs) - (max cost among them)`.

---

# Approach

1. Traverse the string.
2. For every **group of consecutive identical characters**:
   - Calculate the **sum of costs**.
   - Find the **maximum cost**.
   - Add `(sum - max)` to the total answer.
3. Continue until the end.
4. Return the total answer.

---

# Complexity

- **Time Complexity:** `O(N)` → single pass through the string.  
- **Space Complexity:** `O(1)` → only a few variables.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int minimumCost(int n, String s, int[] cost) {
        int totalCost = 0;
        int i = 0;

        while (i < n) {
            char currentChar = s.charAt(i);
            int groupSum = 0;
            int groupMax = 0;

            // Process consecutive characters
            while (i < n && s.charAt(i) == currentChar) {
                groupSum += cost[i];
                groupMax = Math.max(groupMax, cost[i]);
                i++;
            }

            // To make characters unique, remove all except the max one
            totalCost += (groupSum - groupMax);
        }

        return totalCost;
    }

    // Driver for multiple test cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        while (T-- > 0) {
            int n = sc.nextInt();
            String s = sc.next();
            int[] cost = new int[n];
            for (int i = 0; i < n; i++) {
                cost[i] = sc.nextInt();
            }
            System.out.println(minimumCost(n, s, cost));
        }
    }
}
```

---

## **Walkthrough with Example**
Input:  
```
S = "abaac"
COST = [1, 2, 3, 4, 5]
```

Step-by-step:  
- Group 1: `"a"` → cost = 1 → keep it.  
- Group 2: `"b"` → cost = 2 → keep it.  
- Group 3: `"aa"` → costs = [3, 4]  
  - sum = 7, max = 4 → delete cost = 7 - 4 = 3.  
- Group 4: `"c"` → cost = 5 → keep it.  

Total = 3.  

Output = `3`.

---
