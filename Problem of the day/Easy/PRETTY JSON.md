# Intuition

Indentation is driven only by braces and commas. An opening brace starts a new nested block. A closing brace ends a block. A comma ends the current element and forces a new line. Everything inside braces prints with current indentation. Every time you see an opening brace, print it, then increase indentation. Every time you see a closing brace, drop indentation first, then print the brace. Any normal characters accumulate until either a brace or a comma appears.

# Approach

Maintain:
indent to track current depth,
cur for building the current token,
out to store result lines.
Scan the string left to right.
On ‘{’ or ‘[’: flush cur, print brace with current indent, increment indent.
On ‘}’ or ‘]’: flush cur, decrement indent, print brace, add comma if next char is comma.
On ‘,’: append comma to cur, flush and reset cur.
On normal characters: append to cur.
At end, flush cur if non-empty.

---

# Complexity

- **Time complexity:** O(n).
- **Space complexity:** O(n).

---

# Code

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static ArrayList<String> prettyJSON(String str) {
        ArrayList<String> result = new ArrayList<>();
        StringBuilder current = new StringBuilder();
        int indentLevel = 0;
        String indentUnit = "\t";

        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);

            if (ch == '{' || ch == '[') {
                if (current.length() > 0) {
                    result.add(indentUnit.repeat(indentLevel) + current.toString().trim());
                    current.setLength(0);
                }

                result.add(indentUnit.repeat(indentLevel) + ch);
                indentLevel++;
                result.add(indentUnit.repeat(indentLevel));
            } else if (ch == '}' || ch == ']') {
                if (current.length() > 0) {
                    result.add(indentUnit.repeat(indentLevel) + current.toString().trim());
                    current.setLength(0);
                }
                indentLevel--;
                if (i + 1 < str.length() && str.charAt(i + 1) == ',') {
                    result.add(indentUnit.repeat(indentLevel) + ch + ",");
                    i++;
                } else {
                    result.add(indentUnit.repeat(indentLevel) + ch);
                }
            } else if (ch == ',') {
                current.append(ch);
                result.add(indentUnit.repeat(indentLevel) + current.toString().trim());
                current.setLength(0);
            } else {
                current.append(ch);
            }
        }
        return result;
    }
}

```

---

# Example walkthrough

**Input:**
{"A":"B","C":{"D":"E","F":{"G":"H"}}}

Start: indent=0
‘{’ → print “{”, indent=1
“A”: build “A”
“:” → build “A:”
“"B"” → build until comma → cur="A:"B""
‘,’ → print “    A:"B",”

Next element:
“C” → build “C:”
‘{’ → flush “C:”, print with indent=1 → “    C:”
Print “    {” and indent=2

Next “D” → build “D:"E"” until comma → print with indent 2
Next “F” → build “F:”
‘{’ → flush “F:”, print indent 2, print “{”, indent=3

“G:"H"” → flush at comma

**Closing braces unwind:**
indent drops to 2 → print “        }”
indent drops to 1 → print “    }”
indent drops to 0 → print “}”


