# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
    public static int cost(int n, int c, int cuts[]) {

        int[] allCuts = new int[c + 2];
        System.arraycopy(cuts, 0, allCuts, 1, c);
        allCuts[0] = 0;
        allCuts[c + 1] = n;
        Arrays.sort(allCuts);

        int[][] dp = new int[c + 2][c + 2];

        for (int length = 2; length <= c + 1; length++) {
            for (int i = 0; i + length <= c + 1; i++) {
                int j = i + length;
                int minCost = Integer.MAX_VALUE;

                for ( int k = i + 1; k < j; k++) {
                    int cost = allCuts[j] - allCuts[i] + dp[i][k] + dp[k][j];
                    minCost = Math.min(minCost, cost);
                }
                dp[i][j] = minCost;
            }
        }

        return dp[0][c + 1];
    }

}

```