# Intuition

We need to transform a password into a *strong password* by ensuring:

1. Length is between 6 and 20.
2. Contains at least one lowercase, one uppercase, and one digit.
3. No 3 or more consecutive repeating characters.

Operations allowed: **insert, delete, replace**.
We want the **minimum number of changes**.

So we must balance between **fixing length** and **fixing missing character types/repetitions**.

---

# Approach

1. **Check missing character types**:

   * Scan the string and check if it has a lowercase, uppercase, and digit.
   * Let `missingTypes` = number of categories missing (0 to 3).

2. **Check repetitions**:

   * Traverse string and find groups of repeating characters of length ≥ 3.
   * For each group of length `len`, the minimum number of replacements required = `len / 3`.
   * Keep track of these counts.

3. **Adjust based on length**:

   * **Case 1: Length < 6**

     * We must either add characters or fix missing types.
     * Answer = `max(6 - length, missingTypes)`.
   * **Case 2: Length between 6 and 20**

     * No deletions required.
     * Answer = `max(missingTypes, replacementsFromRepetitions)`.
   * **Case 3: Length > 20**

     * Too long → need deletions.
     * Excess = `length - 20`.
     * Deletions can reduce replacements (because deleting characters can break long repeating sequences).

       * First apply deletions on sequences where `(len % 3 == 0)`.
       * Then on `(len % 3 == 1)`.
       * Then on `(len % 3 == 2)`.
     * After applying deletions optimally, compute replacements again.
     * Answer = `excess + max(missingTypes, updatedReplacements)`.

---

# Complexity

* **Time complexity:** `O(n)` (one scan for character checks, one scan for repeats, and adjustments).
* **Space complexity:** `O(n)` in worst case (storing repeat counts), but can be optimized to `O(1)` with inline processing.

---

# Code

```java
public class Solution {
    public static int minChanges(String str) {
        int n = str.length();

        // Step 1: Check missing types
        boolean hasLower = false, hasUpper = false, hasDigit = false;
        for (char c : str.toCharArray()) {
            if (Character.isLowerCase(c)) hasLower = true;
            else if (Character.isUpperCase(c)) hasUpper = true;
            else if (Character.isDigit(c)) hasDigit = true;
        }
        int missingTypes = 0;
        if (!hasLower) missingTypes++;
        if (!hasUpper) missingTypes++;
        if (!hasDigit) missingTypes++;

        // Step 2: Count repeating sequences
        int i = 2;
        java.util.List<Integer> repeats = new java.util.ArrayList<>();
        int replacements = 0;
        for (int j = 0; j < n;) {
            int k = j;
            while (k < n && str.charAt(k) == str.charAt(j)) {
                k++;
            }
            int len = k - j;
            if (len >= 3) {
                repeats.add(len);
                replacements += len / 3;
            }
            j = k;
        }

        // Case 1: Too short
        if (n < 6) {
            return Math.max(6 - n, missingTypes);
        }

        // Case 2: Acceptable length
        if (n <= 20) {
            return Math.max(missingTypes, replacements);
        }

        // Case 3: Too long
        int excess = n - 20;
        int toDelete = excess;
        // Sort repeat lengths to use deletions optimally
        int[] buckets = new int[3];
        for (int len : repeats) {
            buckets[len % 3]++;
        }

        // Apply deletions first to reduce replacements
        for (int mod = 0; mod < 3; mod++) {
            int count = buckets[mod];
            if (mod == 0) {
                // delete 1 char in these sequences
                int use = Math.min(count, toDelete);
                replacements -= use;
                toDelete -= use;
            } else if (mod == 1) {
                // delete 2 chars in these sequences
                int use = Math.min(count * 2, toDelete);
                replacements -= use / 2;
                toDelete -= use;
            } else {
                // mod == 2: delete 3 chars in these sequences
                int use = toDelete / 3;
                replacements -= use;
                toDelete -= use * 3;
            }
        }

        return excess + Math.max(missingTypes, replacements);
    }
}
```

---

# Example Walkthrough

### Example 1

Input: `"a"`

* Length = 1 (< 6).
* Missing types = needs uppercase + digit = 2.
* Must increase length to 6 → need at least 5 insertions.
  Answer = `max(6 - 1, 2) = 5`.

Output: `5` 

### Example 2

Input: `"abcdefg"`

* Length = 7 (okay).
* Missing types = uppercase + digit = 2.
* No repeats.
  Answer = `max(2, 0) = 2`.

Output: `2` 

### Example 3

Input: `"1337C0d"`

* Length = 7 (okay).
* Has digit, uppercase, lowercase. → `missingTypes = 0`.
* No repeats. → `replacements = 0`.
  Answer = `0`.

Output: `0` 

---
