# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static int getMinOperations(int[] finalArray)  {
        int operations = finalArray[0];

        for (int i = 1; i < finalArray.length; i++) {
            if (finalArray[i] > finalArray[i - 1]) {
                operations += (finalArray[i] - finalArray[i - 1]);
            }
        }
        return operations;
    }
}

```