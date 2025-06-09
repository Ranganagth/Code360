# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static int countColumns(String[] strings){
        int n = strings.length;
        int m = strings[0].length();
        int unsortedColumns = 0;

        for (int col = 0; col < m; col++) {
            for (int row = 1; row < n; row++) {
                if (strings[row].charAt(col) < strings[row - 1].charAt(col)) {
                    unsortedColumns++;
                    break;
                }
            }
        }

        return unsortedColumns;
    }
}

```