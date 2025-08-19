# Problem Understanding

This problem is about checking whether we can transform string **K** into string **S** using the special operation:

* Pick one character in **K** and replace **all its occurrences** with another lowercase character (can repeat with other characters).

This essentially boils down to checking if there’s a **consistent one-to-one mapping** between characters in **K** and characters in **S**.

---

# Intuition

When transforming `K` into `S`, every character in `K` must always map to the **same character** in `S`.
For example:

* `K = "aabb"`, `S = "bbcc"`
  `a -> b`, `b -> c` (consistent mapping → possible).

But if one character in `K` maps to two different characters in `S`, transformation is impossible. Similarly, if two characters in `K` must map to the same character in `S` but in conflicting ways, it’s also impossible.

This is essentially checking if there is a **bijective mapping** between characters of `K` and `S`.

---

# Approach

1. If `K.length() != S.length()`, return `false`.
2. Use two hash maps (or arrays of size 26):

   * `mapKtoS` to record mapping from characters of `K` to characters of `S`.
   * `mapStoK` to ensure that two different characters in `K` don’t map to the same character in `S`.
3. Traverse each index `i`:

   * Let `c1 = K.charAt(i)` and `c2 = S.charAt(i)`.
   * If `c1` is already mapped in `mapKtoS`, check consistency with `c2`. If not, create mapping.
   * If `c2` is already mapped in `mapStoK`, check consistency with `c1`. If not, create mapping.
4. If all mappings are consistent, return `true`.
5. Otherwise, return `false`.

---

# Complexity

* **Time Complexity**:
  `O(|K|)` → we process each character once.
* **Space Complexity**:
  `O(1)` → at most 26 lowercase characters in the mapping.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static Boolean canChange(String k, String s) {
        if (k.length() != s.length()) return false;

        Map<Character, Character> mapKtoS = new HashMap<>();
        Set<Character> targetChars = new HashSet<>();

        for (int i = 0; i < k.length(); i++) {
            char c1 = k.charAt(i);
            char c2 = s.charAt(i);

            if (mapKtoS.containsKey(c1)) {
                if (mapKtoS.get(c1) != c2) return false;
            } else {
                mapKtoS.put(c1, c2);
                targetChars.add(c2);
            }
        }

        // If transformation creates cycles, we can only solve them
        // if not all 26 characters are already used as targets.
        if (targetChars.size() < 26) return true;
        else return k.equals(s);
    }
}

```

---

# Example Walkthrough

### Example 1:

```
K = "aabb"
S = "bbcc"
```

* i=0: a→b (new mapping)
* i=1: a→b (consistent)
* i=2: b→c (new mapping)
* i=3: b→c (consistent)
  **Result = True**

### Example 2:

```
K = "ninjas"
S = "coding"
```

* i=0: n→c
* i=1: i→o
* i=2: n→d (conflict, since n was already mapped to c)
  **Result = False**

---
