# Intuition

1. **Regular vs Non-Regular**

   * Regular subsequence = balanced parentheses sequence.
   * Non-regular subsequence = not balanced.

   Example: `"()"` is regular, `"("`, `")"`, `")("`, etc. are non-regular.

2. **Longest non-regular subsequence**

   * If the whole string is already **not regular**, then the **whole string itself** is the longest non-regular subsequence.
   * If the string is regular (balanced), then:

     * Removing **one bracket** from the sequence makes it **non-regular**.
     * So, the longest non-regular subsequence length = `n-1`.

3. **How many distinct subsequences of length n-1?**

   * If `S` is regular:

     * Remove one `'('` → subsequence `S_without_one_left`.
     * Remove one `')'` → subsequence `S_without_one_right`.
     * At most **two distinct subsequences** of length `n-1`.

   * If `S` is already **non-regular**:

     * Only one candidate: the **entire string S**.

4. **Lexicographic Kth**

   * If two candidates exist, sort them lexicographically (`'(' < ')'`).
   * Pick the `Kth`.
   * If `K` > number of candidates, return `-1`.

---

# Approach

1. Write a function `isRegular(s)` using a stack or balance counter.

   * Traverse:

     * `balance++` for `'('`,
     * `balance--` for `')'`.
     * If balance < 0 → invalid.
   * At the end, valid only if `balance == 0`.

2. If `S` is **not regular**:

   * Answer = `S` if `k==1`.
   * Else `-1`.

3. If `S` is **regular**:

   * Generate two subsequences:

     * Remove first `'('`.
     * Remove first `')'`.
   * Add to a `TreeSet` to keep sorted order.
   * If `k` ≤ set size → return kth.
   * Else return `-1`.

---

# Complexity

* **Checking regularity:** `O(n)`
* **Building subsequences:** `O(n)`
* **Sorting candidates:** at most 2 → constant.
* **Total:** `O(n)` per test case.
* Space: `O(n)`

---

# Code

```java
import java.util.*;

public class Solution {

    // check if sequence is regular
    private static boolean isRegular(String s) {
        int balance = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') balance++;
            else balance--;
            if (balance < 0) return false; // too many ')'
        }
        return balance == 0;
    }

    public static String ninjaSubsequence(int n, String s, int k) {
        // case 1: already non-regular
        if (!isRegular(s)) {
            return (k == 1 ? s : "-1");
        }

        // case 2: regular → candidates are two subsequences of length n-1
        List<String> candidates = new ArrayList<>();

        // remove first '('
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') {
                candidates.add(s.substring(0, i) + s.substring(i + 1));
                break;
            }
        }

        // remove first ')'
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == ')') {
                candidates.add(s.substring(0, i) + s.substring(i + 1));
                break;
            }
        }

        // distinct + lexicographic order
        TreeSet<String> set = new TreeSet<>(candidates);

        if (k > set.size()) return "-1";

        int idx = 1;
        for (String cand : set) {
            if (idx == k) return cand;
            idx++;
        }
        return "-1";
    }
}
```

---

## Example Walkthrough

Input:

```
n = 4, s = (()), k = 2
```

* Check `isRegular("(())")` → true.
* Remove first `'('`: → `"())"`.
* Remove first `')'`: → `"(()"`.
* Candidates = { `"(()"`, `"())"` }.
* Lexicographically: `"(()" < "())"`.
* k=2 → `"())"`.

Output:

```
())
```

---
