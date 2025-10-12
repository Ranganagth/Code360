# Intuition

We need to find the **first repeating digit** while scanning from left to right.

The **moment we encounter a digit that we’ve already seen before**, that digit is our answer — because it’s the **first** one to repeat in the order of traversal.

If no digit repeats, return `-1`.

---

# Approach

1. Create a boolean array `seen[10]` since digits range from `0` to `9`.

   * `seen[d] = true` means digit `d` has already been encountered.

2. Traverse the string from left to right:

   * Convert each character to its integer form using `c - '0'`.
   * If `seen[d]` is `true`, it means this digit has appeared before — **return `d`** immediately.
   * Otherwise, mark it as seen → `seen[d] = true`.

3. If traversal finishes without finding any repeating digit, return `-1`.

---

# Complexity

* **Time Complexity:** `O(n)` — single traversal of the string.
* **Space Complexity:** `O(1)` — constant space (array of size 10).

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static int findFirstRepeatingDigit(String digitPattern) {
        boolean[] seen = new boolean[10];

        for (int i = 0; i < digitPattern.length(); i++) {
            int digit = digitPattern.charAt(i) - '0';
            if (seen[digit]) {
                return digit; // first repeating digit found
            }
            seen[digit] = true;
        }
        return -1; // no repeating digit
    }

    // For quick testing
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();
        while (t-- > 0) {
            String s = sc.next();
            System.out.println(findFirstRepeatingDigit(s));
        }
    }
}
```

---

## Example Walkthrough

**Input:** `"456746725"`

| Index | Digit | Seen Before? | Action       | Seen Array After |
| :---- | :---- | :----------- | :----------- | :--------------- |
| 0     | 4     | No           | Mark seen[4] | {4}              |
| 1     | 5     | No           | Mark seen[5] | {4,5}            |
| 2     | 6     | No           | Mark seen[6] | {4,5,6}          |
| 3     | 7     | No           | Mark seen[7] | {4,5,6,7}        |
| 4     | 4     | **Yes**      | Return 4     | —                |

**Output:** `4`

---

