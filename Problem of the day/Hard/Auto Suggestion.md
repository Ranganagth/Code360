# Intuition

After each character is typed, the current prefix grows. For that prefix, only words starting with it are valid suggestions. Among those, at most three lexicographically smallest words are required. Repeating this independently for each prefix is sufficient.

---

# Approach

1. Sort the array `S` lexicographically once.
   This guarantees that any prefix-matching words are already in correct order.
2. Iterate over prefixes of `P` from length `1` to `l`.
3. For each prefix:

   * Scan the sorted array.
   * Collect words that start with the prefix.
   * Stop once three words are collected.
4. If no word matches a prefix, store an empty list for that step.
5. Store the suggestions for each prefix in the result list.

This avoids complex data structures and stays within constraints.

---
# Complexity

**Time Complexity**
* Sorting: `O(N log N)`
* Prefix checks: `O(L * N)` in worst case
  Overall acceptable for given limits.

**Space Complexity**
- O(1) extra space excluding output.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<ArrayList<String>> autoSuggestion(
            int n, String[] S, int l, String P) {

        Arrays.sort(S);

        ArrayList<ArrayList<String>> result = new ArrayList<>();
        String prefix = "";

        for (int i = 0; i < l; i++) {
            prefix += P.charAt(i);
            ArrayList<String> current = new ArrayList<>();

            for (String word : S) {
                if (word.startsWith(prefix)) {
                    current.add(word);
                    if (current.size() == 3) {
                        break;
                    }
                }
            }
            result.add(current);
        }
        return result;
    }
};

```

---

# Example Walkthrough

**Input:**
`S = ["abc", "ab", "aa", "a"]`
`P = "abc"`

After sorting `S`:
`["a", "aa", "ab", "abc"]`

* Prefix `"a"` → matches: `a, aa, ab`
* Prefix `"ab"` → matches: `ab, abc`
* Prefix `"abc"` → matches: `abc`

**Output:**

```
a aa ab
ab abc
abc
```
