# Intuition

We need to verify if all 26 lowercase letters `'a'` to `'z'` are present in the input string, regardless of case. Using a `Set<Character>` is a perfect choice here, as it automatically handles uniqueness.

---

# Approach

1. Convert the string to lowercase.
2. Use a `Set<Character>` to store all unique alphabet characters from the string.
3. If the size of the set is 26, then the sentence is a pangram → return `true`, else `false`.

---

# Complexity

* **Time complexity:** $O(n)$ — where `n` is the length of the string.
* **Space complexity:** $O(1)$ — max 26 characters stored in the set.

---

# Code

```java
import java.util.*;

public class Solution {

    public static boolean ninjaGram(String str) {
        Set<Character> alphabetSet = new HashSet<>();

        for (char ch : str.toLowerCase().toCharArray()) {
            if (ch >= 'a' && ch <= 'z') {
                alphabetSet.add(ch);
            }
        }

        return alphabetSet.size() == 26;
    }
}
```

---
