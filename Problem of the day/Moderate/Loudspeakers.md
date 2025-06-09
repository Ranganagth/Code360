# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    
    public static int[] max_amplitude(int n, int[] loud) {
        
        if ( n == 0) {
            return new int[0];
        }

        int countOnes = 0;
        for (int x : loud) {
            if (x == 1) {
                countOnes++;
            }
        }

        List<Integer> nonOnes = new ArrayList<>();
        for (int x : loud) {
            if (x != 1) {
                nonOnes.add(x);
            }
        }

        Collections.sort(nonOnes);

        int[] result = new int[n];
        int resultIdx = 0;

        if (nonOnes.size() == 2 && nonOnes.get(0) == 2 && nonOnes.get(1) == 3) {
            result[resultIdx++] = 3;
            result[resultIdx++] = 2;
        } else {
            for (int val : nonOnes) {
                result[resultIdx++] = val;
            }
        }

        for (int k = 0; k < countOnes; k++) {
            result[resultIdx++] = 1;
        }

        return result;
    }
}

```