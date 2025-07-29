# Intuition

To determine if a string is a palindrome while ignoring non-alphanumeric characters and case sensitivity, we need to compare characters from both ends of the string. Skipping symbols and spaces ensures only meaningful characters are evaluated. A two-pointer technique is ideal for this with minimal space usage.

---

# Approach

* Use two pointers: one starting from the beginning (`left`) and one from the end (`right`) of the string.
* Skip any non-alphanumeric characters using `Character.isLetterOrDigit()`.
* Convert characters to lowercase using `Character.toLowerCase()` for a case-insensitive comparison.
* If the characters at `left` and `right` don't match, return `false`.
* Continue until both pointers meet or cross. If no mismatches are found, the string is a palindrome.

This approach ensures **O(1) extra space** by avoiding string reconstruction.

---

# Complexity

* Time complexity:
  $O(n)$
  We potentially scan each character at most once from both ends.

* Space complexity:
  $O(1)$
  Only constant space is used for pointers and comparisons.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static boolean checkPalindrome(String str) {
        int left = 0;
        int right = str.length() - 1;

        while (left < right) {
            // Move left pointer if current character is not alphanumeric
            while (left < right && !Character.isLetterOrDigit(str.charAt(left))) {
                left++;
            }

            // Move right pointer if current character is not alphanumeric
            while (left < right && !Character.isLetterOrDigit(str.charAt(right))) {
                right--;
            }

            // Compare characters after converting to lowercase
            if (Character.toLowerCase(str.charAt(left)) != Character.toLowerCase(str.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }

    // For testing multiple inputs
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = Integer.parseInt(sc.nextLine());
        while (T-- > 0) {
            String S = sc.nextLine();
            System.out.println(checkPalindrome(S) ? "Yes" : "No");
        }
    }
}
```

---