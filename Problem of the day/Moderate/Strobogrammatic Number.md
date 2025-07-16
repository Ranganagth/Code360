# Intuitions
> Understand what digits look like when rotated by 180 degrees

Valid strobogrammatic digit mappings:

* `0 → 0`
* `1 → 1`
* `6 → 9`
* `8 → 8`
* `9 → 6`

Invalid digits (can't be used at all):

* `2, 3, 4, 5, 7` — these don’t produce valid digits when rotated.

---

# Approach

We compare digits from both ends toward the center:
* For each `i` and `j = n.length() - 1 - i`, check:
  * Is `n.charAt(i)` a valid digit?
  * Is `rotated(n.charAt(j)) == n.charAt(i)`?

If all match, it is strobogrammatic.

---

# Complexity Analysis

* **Time**: O(N), where N is the number of digits in the string.
* **Space**: O(1), fixed map size.

---

# Code

```java
import java.util.*;

public class Solution {

    public static boolean isStrobogrammatic(String n) {
        Map<Character, Character> map = new HashMap<>();
        map.put('0', '0');
        map.put('1', '1');
        map.put('6', '9');
        map.put('8', '8');
        map.put('9', '6');

        int left = 0;
        int right = n.length() - 1;

        while (left <= right) {
            char l = n.charAt(left);
            char r = n.charAt(right);
            
            if (!map.containsKey(l) || map.get(l) != r) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

---

### **Example Dry Run**

Input: `"8008"`

* '8' maps to '8'
* '0' maps to '0'
* Matches symmetrically → **True**

Input: `"191"`

* '1' maps to '1'
* '9' maps to '6' ≠ '1' → **False**

---
