### Problem Summary

You are given a string `MESSAGE` and a series of operations (left or right rotations) to perform on it in order to **decrypt** it.

* A **left rotation** (`-1`) moves the first character to the end.
* A **right rotation** (`1`) moves the last character to the beginning.

---

# Intuition

Instead of simulating each rotation individually (which would be slow for large inputs), we can:

1. **Calculate the net effect** of all operations.
2. Combine all left and right rotations into a **single effective rotation**.

---

# Approach

#### Step-by-step:

1. Initialize a `netRotation` value.

2. For each operation:
   * If `A == -1`, subtract `B` from `netRotation` (left shift).
   * If `A == 1`, add `B` to `netRotation` (right shift).
   
3. After processing all operations:
   * Normalize the net rotation using modulo with string length:
     `netRotation = netRotation % len
	 `
4. Depending on the final `netRotation`, slice the string:
   * If `netRotation > 0` → right rotation
   * If `netRotation < 0` → left rotation

---
# Complexity Analysis

* **Time**: O(N + |MESSAGE|), where `N` = number of operations
* **Space**: O(1) extra, not including input or output

---
# Code

```java
import java.util.*;

public class Solution {

    public static String decrypt(String message, ArrayList<ArrayList<Integer>> operations) {
        int len = message.length();
        int netRotation = 0;

        for (ArrayList<Integer> op : operations) {
            int dir = op.get(0);
            int steps = op.get(1);

            if (dir == -1) {
                netRotation -= steps;
            } else if (dir == 1) {
                netRotation += steps;
            }
        }

        netRotation %= len; // Normalize within string length

        if (netRotation == 0) return message;

        if (netRotation > 0) { // Right rotation
            return message.substring(len - netRotation) + message.substring(0, len - netRotation);
        } else { // Left rotation
            netRotation = -netRotation;
            return message.substring(netRotation) + message.substring(0, netRotation);
        }
    }
}
```

---

### **Example Walkthrough**

#### Input:

```
message = "abcde"
operations = [
  [-1, 2],
  [1, 1],
  [-1, 2],
  [-1, 1]
]
```

#### Process:

* Initial netRotation = 0
* After `-1 2`: netRotation = -2
* After `1 1`: netRotation = -1
* After `-1 2`: netRotation = -3
* After `-1 1`: netRotation = -4

Now normalize:

* `-4 % 5 = 1` → equivalent to `right rotation by 1`

Final string = `"eabcd"`

---

### **Summary**

* Net rotations avoid simulating each operation
* Works efficiently for large input sizes
* Clean and maintainable