# Solution

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