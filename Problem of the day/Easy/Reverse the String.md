# Intuition

To achieve (O(1)) extra space, the string must be reversed **in-place** by swapping characters symmetrically from both ends toward the center.

---

# Approach

1. Convert the string to a character array.
2. Use two pointers:

   * `left = 0`
   * `right = n-1`
3. Swap characters at `left` and `right`, then move pointers inward.
4. Continue until `left >= right`.
5. Convert the modified array back to string.

---

# Complexity

* **Time:** (O(n))
* **Space:** (O(1)) (ignoring output string construction)

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {	
	public static String reverseString(String str) {
		char[] arr = str.toCharArray();
		int left = 0, right = arr.length - 1;

		while (left < right) {
			char temp = arr[left];
			arr[left] = arr[right];
			arr[right] = temp;
			left++;
			right--;
		}

		return new String(arr);
	}
};

```

---

# Example walkthrough with explanation

Input: `"hello1"`

Array: `[h,e,l,l,o,1]`

Swaps:

* swap `h` and `1` → `[1,e,l,l,o,h]`
* swap `e` and `o` → `[1,o,l,l,e,h]`
* swap `l` and `l` → `[1,o,l,l,e,h]`

Result: `"1olleh"`
