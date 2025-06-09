# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {

    public static boolean countPlatesOnTable(int n,int R,int r) {
        if ( r > R) return false;
        if (n == 1) return true;

        if ( 2 * r > R) return false;

        if ((double) r / (R - r) <= Math.sin(Math.PI / n)) {
            return true;
        }
        return false;

    }
}

```