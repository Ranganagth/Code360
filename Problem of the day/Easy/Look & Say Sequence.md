# Intuition:

To solve the **Look-and-Say Sequence** problem, we need to generate the sequence iteratively by reading the previous term and building the next one based on the frequency and value of digits.

---

# Approach:

1. **Start from "1"** — the first term.
2. For every next term:
   - Read the current term character by character.
   - Count how many times the same digit appears consecutively.
   - Append the count followed by the digit to build the next term.
3. Repeat until we generate the N-th term.

---

# Code:

```java
public class Solution {
    public static String lookAndSaySequence(int n) {
        if (n <= 0) return "";
        
        String result = "1";

        for (int i = 1; i < n; i++) {
            StringBuilder nextTerm = new StringBuilder();
            int count = 1;
            char prevChar = result.charAt(0);

            for (int j = 1; j < result.length(); j++) {
                char currChar = result.charAt(j);
                if (currChar == prevChar) {
                    count++;
                } else {
                    nextTerm.append(count).append(prevChar);
                    count = 1;
                    prevChar = currChar;
                }
            }
            nextTerm.append(count).append(prevChar); // append last group
            result = nextTerm.toString();
        }

        return result;
    }
}
```

---

### **Example:**

For `n = 6`, the sequence will be:

1 → "1"  
2 → "11"  
3 → "21"  
4 → "1211"  
5 → "111221"  
6 → **"312211"**

---
