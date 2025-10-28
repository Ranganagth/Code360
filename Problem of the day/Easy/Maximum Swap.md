# Intuition

We can **swap two digits at most once** to get the **largest possible number**.

So, we need to find:

* **Where** to swap, and
* **Which digits** to swap
  to make the number as large as possible.

The goal is to make the **most significant digit** (leftmost) as large as possible by swapping it with a **larger digit appearing later**.

---

# Approach

### Step 1: Convert number to digits array

Convert `n` into a character array (or digit array), since swapping digits is easier.

### Step 2: Track the last occurrence of each digit (0–9)

Create an array `last[10]` where `last[d]` stores the **last index** where digit `d` appears in the number.
This helps us quickly know **where the largest available digit** is located later in the number.

### Step 3: Traverse from left to right

For each position `i`:

* Let the current digit be `curr = digits[i]`
* Check if there exists a **larger digit (9 down to curr+1)** that appears **after** `i`
* If found, **swap** the current digit with that larger digit’s **last occurrence**
* Return the number after swap (since we can only swap once)

If no swap is possible, return the original number.

---

# Complexity

* **Time:** O(d) where `d` = number of digits (≤ 10 for 10^9)
* **Space:** O(1)

---

# Code

```java
import java.util.*;

public class Solution {
    public static int maximumSwap(int n) {
        char[] digits = String.valueOf(n).toCharArray();
        int[] last = new int[10];

        // Record the last occurrence of each digit
        for (int i = 0; i < digits.length; i++) {
            last[digits[i] - '0'] = i;
        }

        // Try to find the first place to swap
        for (int i = 0; i < digits.length; i++) {
            int curr = digits[i] - '0';
            // Look for a larger digit that appears later
            for (int d = 9; d > curr; d--) {
                if (last[d] > i) {
                    // Swap
                    char temp = digits[i];
                    digits[i] = digits[last[d]];
                    digits[last[d]] = temp;
                    // Return the new number
                    return Integer.parseInt(new String(digits));
                }
            }
        }

        // No swap can increase the number
        return n;
    }
};

```

---

## Example Walkthrough

### Example 1

```
n = 4589
```

**Digits:** [4, 5, 8, 9]

**Last occurrence map:**

```
Digit 4 -> 0
Digit 5 -> 1
Digit 8 -> 2
Digit 9 -> 3
```

Now check each digit:

* For `4`:
  There exists a higher digit (9) **after index 0**.
  Swap 4 ↔ 9 → [9, 5, 8, 4] → **9584** 
  That’s the maximum.

No need to check further (since only one swap allowed).

---

### Example 2

```
n = 99538
```

**Digits:** [9, 9, 5, 3, 8]

**Last occurrence map:**

```
Digit 9 -> 1
Digit 8 -> 4
Digit 5 -> 2
Digit 3 -> 3
```

* i = 0 → 9 (already largest)
* i = 1 → 9 (still largest)
* i = 2 → 5
  Larger digits? Yes — 8 (index 4)
  Swap 5 ↔ 8 → [9, 9, 8, 3, 5] → **99835** 

---

### Example 3

```
n = 4321
```

All digits already decreasing → **No swap increases value**
Return 4321.

---

## Example Validation

| Input | Output | Explanation     |
| ----- | ------ | --------------- |
| 4589  | 9584   | Swap 4 ↔ 9      |
| 99538 | 99835  | Swap 5 ↔ 8      |
| 4321  | 4321   | Already largest |
| 18    | 81     | Swap 1 ↔ 8      |

---

