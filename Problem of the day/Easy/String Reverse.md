# Intuition

Reversing a string means swapping characters from both ends toward the center. For example:

* `"hello"` → swap `h` and `o`, then `e` and `l` → `"olleh"`.

---

# Approach

1. Convert the input string into a `char[]` (already given in the function).
2. Use two pointers: `i = 0` and `j = arr.length - 1`.
3. While `i < j`, swap `arr[i]` and `arr[j]`.
4. After the loop, convert the array back to a `String`.

---

# Complexity

* **Time:** O(N) (each character visited once).
* **Space:** O(1) extra space (in-place swaps).

---

# Code

```java
import java.util.* ;
import java.io.*; 

class Solution {

  public static String stringReverse(char[] arr) {
    int i = 0, j = arr.length - 1;
    while (i < j) {
      char temp = arr[i];
      arr[i] = arr[j];
      arr[j] = temp;
      i++;
      j--;
    }
    return new String(arr);
  }
}
```

---

## Walkthrough with Example

Input: `"ninja"`

* Initial: `['n','i','n','j','a']`
* Swap `n` and `a` → `['a','i','n','j','n']`
* Swap `i` and `j` → `['a','j','n','i','n']`
* Done → `"ajnin"`

Input: `"MoM"` → swapping results in `"MoM"` (same, palindrome).

---
