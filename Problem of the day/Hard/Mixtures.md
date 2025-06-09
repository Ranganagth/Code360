# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
   public static int minimiseSmoke(ArrayList<Integer> color, int n) {
       int[][] dp = new int[n][n];
       int[][] sum = new int[n][n];

       for (int i = 0; i <n; i++) {
           sum[i][i] = color.get(i);
       }

       for (int len = 2; len <= n; len++) {
           for (int i = 0; i <= n - len; i++) {
               int j = i + len -1;
               dp[i][j] = Integer.MAX_VALUE;

               for(int k = i; k < j; k++) {
                   int smoke = dp[i][k] +dp[k + 1][j] + sum[i][k] * sum[k +1][j];
                   if (smoke < dp[i][j]) {
                       dp[i][j] = smoke;
                       sum[i][j] = (sum[i][k] + sum[k + 1][j]) % 100;
                   }
               }
           }
       }
   return dp[0][n - 1];
   }

   public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
       int T = scanner.nextInt();

       while (T-- > 0) {
           int n = scanner.nextInt();
           ArrayList<Integer> color = new ArrayList<>();

           for (int i = 0; i < n; i++) {
               color.add(scanner.nextInt());
           }
           System.out.println(minimiseSmoke(color, n));
       }
       scanner.close();
   }
}

```