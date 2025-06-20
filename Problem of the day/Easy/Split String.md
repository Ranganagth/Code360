# Intuition

To determine if both halves of a string contain the same number of vowels, we simply:

* Split the string into two equal halves.
* Count the vowels in each half.
* Compare the two counts.

---

# Approach

1. Convert the string to lowercase to ensure case-insensitive vowel matching.
2. Use a helper function to count vowels in each half.
3. Compare the counts and return the result.

---

# Complexity

* **Time complexity:** O(n), where n is the length of the string.
* **Space complexity:** O(1), constant extra space for counting.

---

# Code

```java
import java.util.*;

public class Solution {
    public static Boolean splitString(String str) {
        int n = str.length();
        int mid = n / 2;
        int count1 = 0, count2 = 0;

        for (int i = 0; i < mid; i++) {
            if (isVowel(str.charAt(i))) {
                count1++;
            }
        }

        for (int i = mid; i < n; i++) {
            if (isVowel(str.charAt(i))) {
                count2++;
            }
        }

        return count1 == count2;
    }

    private static boolean isVowel(char ch) {
        ch = Character.toLowerCase(ch);
        return ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u';
    }
}
```

---

### Example Execution

For input:

```
2
codingninjas
helloworld
```

* `"codingninjas"` → `"coding"` and `"ninjas"` both have 2 vowels → **True**
* `"helloworld"` → `"hello"` (2 vowels) and `"world"` (1 vowel) → **False**

---
