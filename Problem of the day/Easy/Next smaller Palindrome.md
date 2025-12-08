# Intuition:

The given number is already a palindrome. To get the next smaller palindrome, reduce the palindrome while keeping symmetry. Copy left side to right. If the mirrored value is already smaller, return it. Otherwise, decrement the middle digit(s), handle borrow, then mirror again. Special case: numbers like "11", "101", "1001" collapse to all 9s of length N−1.

---

# Approach:

1. If the string is all zeros or single-digit, return the previous digit.
2. Check if the number is of form `1000...0001`. If yes, answer is all `'9'`s with length = `n-1`.
3. Mirror left half to right.
4. If the mirrored value is already smaller than input, return.
5. Otherwise decrement the middle element(s):

   * If odd length: decrement the middle digit.
   * If even length: decrement the two middle digits.
6. If borrow occurs, propagate to the left and mirror after adjustment.

---

# Complexity:

- **Time:** `O(n)`
- **Space:** `O(n)`

---

# Code:

```java
import java.util.*;

public class Solution {
    public static String nextSmallerPalindrome(String s) {
        int n = s.length();
        
        // Single digit case
        if (n == 1) {
            int x = s.charAt(0) - '0';
            return String.valueOf(x - 1);
        }

        // Check if the number is like 100...001
        boolean isBoundary = true;
        for (int i = 1; i < n - 1; i++) {
            if (s.charAt(i) != '0') {
                isBoundary = false;
                break;
            }
        }
        if (isBoundary && s.charAt(0) == '1' && s.charAt(n - 1) == '1') {
            // Return all 9s of length n-1
            return "9".repeat(n - 1);
        }

        char[] arr = s.toCharArray();

        // Mirror left to right first
        for (int i = 0; i < n / 2; i++) {
            arr[n - 1 - i] = arr[i];
        }

        String mirrored = new String(arr);
        if (mirrored.compareTo(s) < 0) {
            return mirrored;
        }

        // Need to decrement the middle part
        int left = (n - 1) / 2;

        while (left >= 0 && arr[left] == '0') {
            arr[left] = '9';
            left--;
        }

        arr[left]--; // borrow adjustment

        // Mirror again
        for (int i = 0; i < n / 2; i++) {
            arr[n - 1 - i] = arr[i];
        }

        // Remove leading zeros except result = "0"
        String res = new String(arr);
        if (res.charAt(0) == '0') {
            return "9".repeat(n - 1);
        }

        return res;
    }
}
```

---

# Example Walkthrough:

**Input:** `12321`
Mirror → `12321` (not smaller)
Decrease middle `3 → 2` → now `"12221"`
Mirror ensures palindrome.
**Output:** `12221`
