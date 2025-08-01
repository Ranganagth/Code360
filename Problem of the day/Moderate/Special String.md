# Intuition

We are given a string `S` of length `N`, composed of the first `P` lowercase English letters (`a` to `a + p - 1`). 

The string is _special_, meaning:
- No palindrome substrings of length ≥ 2
    - That means:
        - `s[i] != s[i - 1]` (no 2-length palindrome)            
        - `s[i] != s[i - 2]` (no 3-length palindrome)

We are to find the **next lexicographically greater string** that is also special and of the **same length**. If it doesn't exist, return `"NO"`.

---
# Approach

We simulate **lexicographical incrementing** of string `S` from the **last index to first**, trying to **increment** one character while ensuring the result is still special.

At each position `i`, we:
1. Try increasing the character at index `i` from its current value up to `'a' + p - 1`
2. For each such candidate:
    - Ensure it does **not form a palindrome** with previous two characters (if available)
3. If a valid character is found at position `i`, we:
    - Fix it
    - Fill the rest of the string (`i + 1` to `N - 1`) with the **smallest valid characters** that do not break the special string rule

If no such configuration is found, return `"NO"`

To solve this problem, we need to **generate the lexicographically next string** (larger than `s`) of length `n`, using only the first `p` lowercase letters (i.e., `'a'` to `(char)('a' + p - 1)`), and **ensure it contains no palindromic substrings** of length ≥ 2.

### Key Observations:

* A palindrome of length 2: `s[i] == s[i-1]`
* A palindrome of length 3: `s[i] == s[i-2]`
* So we must ensure that for any `i ≥ 1`: `s[i] != s[i-1]` and for `i ≥ 2`: `s[i] != s[i-2]`

We use a **backtracking-style loop from the end** to attempt finding the next valid lexicographical string. Once we find a valid character to increment, we try to build the remaining suffix with the **smallest valid characters** that satisfy the non-palindrome condition.

---

# Time and Space Complexity

- **Time Complexity**: `O(N * P)` per test case — at each character position, we try up to `P` characters.

- **Space Complexity**: `O(N)` to construct the result string.    


---

# Code

```java
import java.util.*;

public class Solution {
    public static String specialString(String s, int n, int p) {
        char[] ch = s.toCharArray();

        for (int i = n - 1; i >= 0; i--) {
            for (char c = (char)(ch[i] + 1); c < (char)('a' + p); c++) {
                if (isValid(ch, i, c)) {
                    ch[i] = c;
                    if (fillRemaining(ch, i + 1, p)) {
                        return new String(ch);
                    }
                }
            }
        }

        return "NO";
    }

    private static boolean isValid(char[] ch, int idx, char c) {
        if (idx >= 1 && ch[idx - 1] == c) return false;
        if (idx >= 2 && ch[idx - 2] == c) return false;
        return true;
    }

    private static boolean fillRemaining(char[] ch, int start, int p) {
        for (int i = start; i < ch.length; i++) {
            boolean found = false;
            for (char c = 'a'; c < (char)('a' + p); c++) {
                if (isValid(ch, i, c)) {
                    ch[i] = c;
                    found = true;
                    break;
                }
            }
            if (!found) return false;
        }
        return true;
    }
}
```

---
### Example Walkthrough

#### Input:

```
N = 4, P = 4
S = "abcd"
```

* Characters allowed: `'a', 'b', 'c', 'd'`
* Start from right:
  * Try `d` → `e` → invalid (out of bound)
  * Try changing `'d'` to `'a'`, `'b'`, ..., all fails due to lex order
* Move to position 2 (`'c'`)
  * Try `'d'` → new string: `"abd"`
    * Fill next: try `'a'`, passes check → final string: `"abda"` 
* So result is **`abda`**

---

### Sample Input/Output

#### Input:

```
2
4 4
abcd
4 4
dcbd
```

#### Output:

```
abda
NO
```

---


### Explanation:

* `specialString`: Iterates backward through the string, trying the next possible character at position `i`, and then attempts to fill the rest of the string with the smallest valid characters.
* `isValid`: Ensures that placing character `c` at position `idx` won’t form a palindrome.
* `fillRemaining`: Tries to fill the rest of the array from `start` to end with the smallest valid characters.

