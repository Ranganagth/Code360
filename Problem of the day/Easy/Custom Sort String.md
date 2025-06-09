# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static String specificOrder(String x, String y) {
        Map<Character, Integer> freqMap = new HashMap<>();

        for (char ch : x.toCharArray()) {
            freqMap.put(ch, freqMap.getOrDefault(ch, 0) + 1);
        }

        StringBuilder result = new StringBuilder();

        for (char ch : y.toCharArray()) {
            if (freqMap.containsKey(ch)) {
                int count = freqMap.get(ch);
                for (int i = 0; i < count; i++) {
                    result.append(ch);
                }
                freqMap.remove(ch);
            }
        }

        for (Map.Entry<Character, Integer> entry : freqMap.entrySet()) {
            char ch = entry.getKey();
            int count = entry.getValue();
            for (int i = 0; i < count; i++) {
                result.append(ch);
            }
        }

        return result.toString();
    }
}

```