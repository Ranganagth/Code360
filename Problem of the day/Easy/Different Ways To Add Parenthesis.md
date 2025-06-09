# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static int[] diffWaysToCompute(String s) {
        List<Integer> result = compute(s);

        int[] resArray = new int[result.size()];
        for (int i = 0; i < result.size(); i++) {
            resArray[i] = result.get(i);
        }

        return resArray;
    }

    private static List<Integer> compute(String s) {
        List<Integer> results = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);

            if (ch == '+' || ch == '-' || ch == '*') {
                String left = s.substring(0, i);
                String right = s.substring(i + 1);

                List<Integer> leftResults = compute(left);
                List<Integer> rightResults = compute(right);

                for (int l : leftResults) {
                    for (int r : rightResults) {
                        int val = 0;
                        if (ch == '+') {
                            val = l + r;
                        } else if (ch == '-'){
                            val = l - r;
                        } else if (ch == '*') {
                            val = l * r;
                        }
                        results.add(val);
                    }
                }
            }
        }

        if (results.isEmpty()) {
            results.add(Integer.parseInt(s));
        }
        return results;
    }
}

```