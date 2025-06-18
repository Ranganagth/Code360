# Intuition

To find the first non-repeating character, we need to know two things:

1. How many times each character appears in the string.
2. The order in which the characters appear.

Using a frequency map preserves character counts, and we can iterate the string again to find the first character with a count of 1.

---

# Approach

1. Use an integer array `freq` of size 26 to count occurrences of each character (since the string contains only lowercase letters).
2. Traverse the string to fill the `freq` array.
3. Traverse the string a second time to find the first character with frequency 1.
4. If none found, return `#`.

This method avoids using more complex data structures like `LinkedHashMap`, making it space and time efficient.

---

# Complexity

* **Time complexity:**
  $O(n)$ for each string, where $n$ is the length of the string.

* **Space complexity:**
  $O(1)$ since the frequency array size is fixed at 26 (constant space for lowercase English letters).

---

# Code

```java
import java.util.*;

public class Solution {
    public static char firstNonRepeating(String str) {
        int[] freq = new int[26];

        for (char ch : str.toCharArray()) {
            freq[ch - 'a']++;
        }

        for (char ch : str.toCharArray()) {
            if (freq[ch - 'a'] == 1) {
                return ch;
            }
        }

        return '#';
    }

    // Optional: main method for testing multiple test cases
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = sc.nextInt();
        sc.nextLine(); // Consume leftover newline
        while (T-- > 0) {
            String str = sc.nextLine();
            System.out.println(firstNonRepeating(str));
        }
    }
}
```
