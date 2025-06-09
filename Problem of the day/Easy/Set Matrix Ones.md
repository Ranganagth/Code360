# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution
{
    public static void setMatrixOnes(ArrayList<ArrayList<Integer>> MAT, int n, int m)
    {
        boolean[] rowMakers = new boolean[n];
        boolean[] colMakers = new boolean[m];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (MAT.get(i).get(j) == 1) {
                    rowMakers[i] = true;
                    colMakers[j] = true;
                }
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (rowMakers[i] || colMakers[j]) {
                    MAT.get(i).set(j, 1);
                }
            }
        }
    }
}

```