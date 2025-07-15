# Intuition

Ninja starts with **no change**. For each customer:

* If they pay **\$5**, keep it (no change needed).
* If they pay **\$10**, check if Ninja has at least one **\$5** to return as change.
* If they pay **\$20**, check if Ninja can return **\$15** using:
  * One **\$10** and one **\$5** (preferred), or
  * Three **\$5** notes (fallback).

If at any point Ninja cannot give the required change, return `false`.

---

# Approach

* Use two counters: `five` and `ten` to track number of `$5` and `$10` notes.
* Traverse each bill and update counters or check for possible change.
* If not possible to give change, return `false`.

---

# Complexity

* **Time Complexity:** `O(N)` per test case (where N is size of `bill`)
* **Space Complexity:** `O(1)` — only using two variables

---

# Code

```java
import java.util.*;

public class Solution {
    public static boolean ninjaShikanji(ArrayList<Integer> bill, int n) {
        int five = 0, ten = 0;

        for (int i = 0; i < n; i++) {
            int amount = bill.get(i);

            if (amount == 5) {
                five++;
            } else if (amount == 10) {
                if (five == 0) return false;
                five--;
                ten++;
            } else if (amount == 20) {
                if (ten > 0 && five > 0) {
                    ten--;
                    five--;
                } else if (five >= 3) {
                    five -= 3;
                } else {
                    return false;
                }
            }
        }

        return true;
    }
}
```

---

### **Example Dry Run**

**Input:**
`bill = [5, 10, 5, 20, 5]`

**Step-by-step:**

* Pay 5 → keep (five = 1)
* Pay 10 → give one 5 (five = 0, ten = 1)
* Pay 5 → keep (five = 1)
* Pay 20 → give 10+5 (ten = 0, five = 0)
* Pay 5 → keep (five = 1)

All succeeded → return `true`

---

### **Another Example**

**Input:**
`bill = [5, 20, 5, 10]`

**Step-by-step:**

* Pay 5 → keep (five = 1)
* Pay 20 → can’t give change (need 15, have only one 5)

Return `false`

---
