# Intuition

Two strings are **isomorphic** if you can map every character in `str1` to a **unique** character in `str2` such that the mapping is consistent across the entire string.

That means:

* Every occurrence of a character in `str1` should map to the same character in `str2`.
* Two different characters in `str1` cannot map to the same character in `str2`.
* The order of mapping must remain consistent.

For example:

```
str1 = "aab", str2 = "xxy"
'a' → 'x'
'b' → 'y'
=> valid mapping → return true
```

---

# Approach

1. If lengths of `str1` and `str2` are different → return `false`.
2. Use **two hash maps (or arrays)** to keep track of mappings:

   * `map1` for characters in `str1 → str2`
   * `map2` for characters in `str2 → str1`
3. Iterate over the strings:

   * For each index `i`, get `c1 = str1.charAt(i)` and `c2 = str2.charAt(i)`.
   * If `c1` was already mapped, check if it maps to the same `c2`.
     If not → return `false`.
   * If `c2` was already mapped to some other `c1` → return `false`.
   * Otherwise, store the mapping in both directions.
4. If the loop completes, return `true`.

## Algorithm

```text
if (lengths differ) return false

create two arrays of size 256 (ASCII characters) for mapping:
map1 = new int[256]
map2 = new int[256]

for each i in 0..n-1:
    c1 = str1.charAt(i)
    c2 = str2.charAt(i)
    
    if map1[c1] == 0 and map2[c2] == 0:
        map1[c1] = c2
        map2[c2] = c1
    else if map1[c1] != c2:
        return false

return true
```

---

# Complexity

* **Time Complexity:** O(N)
* **Space Complexity:** O(1) (constant 256 array size for ASCII)

---

# Code

```java
public class Solution {
    public static boolean areIsomorphic(String str1, String str2) {
        if (str1.length() != str2.length()) return false;

        int[] map1 = new int[256];
        int[] map2 = new int[256];

        for (int i = 0; i < str1.length(); i++) {
            char c1 = str1.charAt(i);
            char c2 = str2.charAt(i);

            if (map1[c1] == 0 && map2[c2] == 0) {
                map1[c1] = c2;
                map2[c2] = c1;
            } else if (map1[c1] != c2) {
                return false;
            }
        }

        return true;
    }
};

```

---

## Example Walkthrough

### Example 1:

```
str1 = "aab"
str2 = "xxy"

i=0: map 'a' → 'x'
i=1: 'a' again → 'x' (consistent)
i=2: map 'b' → 'y'

valid mapping → return true
```

### Example 2:

```
str1 = "aab"
str2 = "xyz"

i=0: map 'a' → 'x'
i=1: 'a' again → 'x' (ok)
i=2: map 'b' → 'z'

 but there’s no corresponding 'y' → mapping inconsistent → return false
```

**Output:**

```
Input:
aab
xxy
Output: 1 (true)

Input:
aab
xyz
Output: 0 (false)
```
