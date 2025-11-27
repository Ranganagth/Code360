# Intuition

A strobogrammatic number remains the same when rotated **180 degrees**. Only specific digit pairs remain valid after rotation:

* `0 ↔ 0`
* `1 ↔ 1`
* `8 ↔ 8`
* `6 ↔ 9`
* `9 ↔ 6`

The problem reduces to constructing numbers from the **outside inward**, placing valid pairs symmetrically.

For odd lengths, the middle digit must be one of: `0`, `1`, `8`, since these are the only digits symmetric with themselves.

We recursively build possibilities by filling positions `left` and `right` until they cross.

---

# Approach

1. Maintain a list of valid strobogrammatic pairs.
2. Use a recursion/backtracking function:

   * Place a valid pair at `left` and `right`.
   * Move inward (`left + 1`, `right - 1`).
3. Constraints to enforce:

   * The first digit cannot be `0` unless the number length is `1`.
   * When `left == right`, only symmetric digits are allowed (`0,1,8`).
4. When `left > right`, we have a valid number → add to result.

---

# Complexity

* **Time Complexity:**
  Each level branches up to 5 possibilities → approximately:
  $$  O(5^{N/2})$$

* **Space Complexity:**
  Recursion depth and array storage:
  $$O(N)$$
  

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    
    private static final char[][] PAIRS = {
        {'0', '0'},
        {'1', '1'},
        {'6', '9'},
        {'8', '8'},
        {'9', '6'}
    };

    public static ArrayList<String> findStrobogrammatic(int n) {
        ArrayList<String> result = new ArrayList<>();
        char[] current = new char[n];
        generate(0, n - 1, current, result, n);
        return result;
    }

    private static void generate(int left, int right, char[] current, ArrayList<String> result, int length) {

        if (left > right) {
            result.add(new String(current));
            return;
        }

        for (char[] pair : PAIRS) {
            
            if (left == 0 && length > 1 && pair[0] == '0') continue;
            if (left == right && pair[0] != pair[1]) continue;

            current[left] = pair[0];
            current[right] = pair[1];

            generate(left + 1, right - 1, current, result, length);
        }
    }
};

```

---

# Example Walkthrough

**Input:**

```
N = 3
```

Positions:

```
[ _ , _ , _ ]
 0       2
```

Step-by-step:

1. Try placing each valid pair at (0,2):

   * `0,0` → Not allowed (leading zero)
   * `1,1` → `[1 _ 1]`
   * `6,9` → `[6 _ 9]`
   * `8,8` → `[8 _ 8]`
   * `9,6` → `[9 _ 6]`

2. Now middle index = `1`, allowed digits only (`0,1,8`):

   For `[1 _ 1]`:

   * `101`, `111`, `181`

   For `[6 _ 9]`:

   * `609`, `619`, `689`

   For `[8 _ 8]`:

   * `808`, `818`, `888`

   For `[9 _ 6]`:

   * `906`, `916`, `986`

**Final Output:**

```
101 111 181 609 619 689 808 818 888 906 916 986
```

---

