# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution 
{
    public static int distinctStrings(ArrayList<String> arr, int n)
    {
        Set<String> uniqueSignatures = new HashSet<>();

        for (String s : arr) {
            List<Character> even = new ArrayList<>();
            List<Character> odd = new ArrayList<>();

            for (int i = 0; i < s.length(); i++) {
                if (i % 2 == 0) {
                    even.add(s.charAt(i));
                } else {
                    odd.add(s.charAt(i));
                }
            }

            Collections.sort(even);
            Collections.sort(odd);

            StringBuilder signature = new StringBuilder();
            for (char ch : even) signature.append(ch);
            signature.append('#');
            for (char ch : odd) signature.append(ch);

            uniqueSignatures.add(signature.toString());
        }

        return uniqueSignatures.size();
    }
}

```