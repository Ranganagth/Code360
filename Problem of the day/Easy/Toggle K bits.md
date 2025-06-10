# Intuition

To toggle (flip) the rightmost `K` bits of an integer `N`:

* Construct a mask with `K` 1's at the rightmost positions. For example, if `K = 3`, then mask is `000...0111` (binary).
* Use **XOR (^)** with this mask:

  * XOR flips bits where the mask is 1, leaves others unchanged.

---

# Approach

1. **Create the mask**: `(1 << K) - 1`
   This gives K rightmost 1s.
2. **XOR the mask with `N`**: `N ^ mask`
3. Return the result.

# Complexity

* **Time**: O(1) (constant time bitwise operation)
* **Space**: O(1)
---

# Code

```java
public class Solution {
    public static int toggleKBits(int n, int k) {
        int mask = (1 << k) - 1;
        return n ^ mask;
    }
}
```

---

# Example Walkthrough

#### Input: `n = 21`, `k = 3`

* Binary: `21 = 10101`
* Mask: `000111` (i.e., 7)
* XOR: `10101 ^ 00111 = 10010` → `18`

---
