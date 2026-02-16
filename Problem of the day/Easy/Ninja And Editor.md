# Intuition

In the corrupted string, every uppercase letter (except possibly the first character) indicates the beginning of a new word. Converting the string back requires:

* inserting a space before each uppercase letter (except at index 0),
* converting all characters to lowercase.

# Approach

1. Traverse the string character by character.
2. If the current character is uppercase and not at index `0`, append a space before it.
3. Append the lowercase version of the character.
4. Return the constructed string.

# Complexity

* Time complexity: (O(n))
* Space complexity: (O(n))

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static String editSentence(String str){
        if (str == null || str.length() == 0) return "";

        StringBuilder res = new StringBuilder();

        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);

            if (Character.isUpperCase(ch) && i != 0) {
                res.append(' ');
            }
            res.append(Character.toLowerCase(ch));
        }

        return res.toString();
    }
}
```

# Example walkthrough with explanation

Input: `"CodingNinjasIsACodingPlatform"`

Processing:

* `C` → `c`
* `N` → insert space → ` ninjas`
* `I` → insert space → ` is`
* `A` → insert space → ` a`
* `C` → insert space → ` coding`
* `P` → insert space → ` platform`

Result: `"coding ninjas is a coding platform"`
