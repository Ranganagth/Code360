# Intuition

The task is straightforward string processing:

* Every **space character `' '`** in the given string must be replaced with the string **`"@40"`**.
* All other characters remain unchanged.
* The input is a `StringBuilder`, so we should **modify and return a `StringBuilder`**, not a `String`.

---

# Approach

Iterate through the characters of the `StringBuilder`:

* If the current character is a space:

  * Replace it with `"@40"`
  * Since the replacement is longer than one character, adjust the index accordingly
* Otherwise, move to the next character

Using `StringBuilder.replace()` is efficient and clean.

---

# Complexity

- **Time Complexity**
	* **O(N)**
  Where `N` is the length of the string

- **Space Complexity**
	* **O(1)** extra space
  (Modification is done in-place using `StringBuilder`)

---

# Code

```java
public class Solution {
    public static StringBuilder replaceSpaces(StringBuilder str) {

        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == ' ') {
                str.replace(i, i + 1, "@40");
                i += 2; // Move past inserted "@40"
            }
        }

        return str;
    }
};

```

---

# Example Walkthrough

#### Input

```
"Coding Ninjas Is A Coding Platform"
```

#### Step-by-step

* `' '` → replaced with `"@40"`
* All spaces converted

#### Output

```
Coding@40Ninjas@40Is@40A@40Coding@40Platform
```

---

### Key Notes

* Works correctly for:

  * Strings with no spaces
  * Multiple spaces
  * Empty strings
* Uses **in-place modification**, as required

This solution is optimal, clean, and adheres strictly to the problem constraints.
