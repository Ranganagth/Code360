# Intuition

The problem is essentially about **reversing the order of words** in a string while cleaning up extra spaces.
Key points:

* Words are separated by one or more spaces.
* Output should have exactly one space between words.
* No leading or trailing spaces in the result.

The challenge is both parsing the words correctly and then reversing their order.

---

# Approach

1. **Trim spaces**: First, remove leading and trailing spaces from the string.
2. **Split words**: Use regex split (`split("\\s+")`) to separate words by one or more spaces. This automatically handles multiple spaces.
3. **Reverse words**: Store them in an array, then reverse the order.
4. **Join words**: Join the reversed array with a single space `" "`.

This ensures:

* Extra spaces are removed.
* Words are reversed in order.
* Output is clean and valid.

For the **follow-up (in-place with O(1) space)**:

* Convert string to a mutable array (like `char[]` in Java).
* Reverse the entire array.
* Then reverse each word individually.
* Finally, remove extra spaces.
  But since Java `String` is immutable, the simple split-and-reverse solution is optimal and cleaner.

---

# Complexity

* **Time complexity**:

  * Splitting the string: `O(N)`
  * Reversing words: `O(N)`
  * Joining words: `O(N)`
  * Overall: **O(N)**
* **Space complexity**:

  * Extra space for storing words: **O(N)**

---

# Code

```java
public class Solution {
    public static String reverseString(String str) {
        // Step 1: Trim leading/trailing spaces
        str = str.trim();
        
        // Step 2: Split by one or more spaces
        String[] words = str.split("\\s+");
        
        // Step 3: Reverse the words
        StringBuilder result = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            result.append(words[i]);
            if (i > 0) {
                result.append(" ");
            }
        }
        
        return result.toString();
    }
}
```

---

# Example walkthrough with explanation

Input:

```
"   Welcome   to   Coding   Ninjas   "
```

1. **Trim** → `"Welcome   to   Coding   Ninjas"`
2. **Split** → `["Welcome", "to", "Coding", "Ninjas"]`
3. **Reverse** → `["Ninjas", "Coding", "to", "Welcome"]`
4. **Join** → `"Ninjas Coding to Welcome"`

Output:

```
"Ninjas Coding to Welcome"
```

---

