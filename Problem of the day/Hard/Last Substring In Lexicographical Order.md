# Intuition

The lexicographically last substring must start at the position where the suffix of the string is lexicographically maximum.
Thus the problem reduces to **finding the lexicographically largest suffix** of the string.
This can be done in linear time using a two-pointer comparison method (similar to Booth / maximal suffix algorithm).

---

# Approach

Maintain two candidate starting indices:

* `i` → current best suffix
* `j` → next candidate suffix
* `k` → offset used to compare characters

Process:

1. Compare `str[i + k]` and `str[j + k]`.
2. If equal → increment `k`.
3. If `str[i + k] > str[j + k]` → suffix at `j` is worse, move `j = j + k + 1`.
4. If `str[i + k] < str[j + k]` → suffix at `i` is worse, move `i = max(i + k + 1, j)`, then `j = i + 1`.
5. Reset `k = 0` after every mismatch.
6. When `j` reaches end, suffix at `i` is the lexicographically largest.

Return substring starting at `i`.

---

# Complexity

* **Time:** (O(n))
* **Space:** (O(1))

---

# Code

```java
public class Solution 
{
    public static String findLastSubstring(String str) 
    {
        int n = str.length();
        int i = 0, j = 1, k = 0;

        while (j + k < n) 
        {
            char a = str.charAt(i + k);
            char b = str.charAt(j + k);

            if (a == b) 
            {
                k++;
            } 
            else if (a > b) 
            {
                j = j + k + 1;
                k = 0;
            } 
            else 
            {
                i = Math.max(i + k + 1, j);
                j = i + 1;
                k = 0;
            }
        }

        return str.substring(i);
    }
};

```

---

# Example walkthrough with explanation

For `"abba"`:

Suffixes:

* `"abba"`
* `"bba"`
* `"ba"`
* `"a"`

Comparison process eliminates smaller suffixes step by step, leaving start index `1`.

Result substring: `"bba"`.
