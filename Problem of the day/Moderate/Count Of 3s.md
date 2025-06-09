# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
  public static long countOf3(int x) {
      long count = 0;

      for (int i = 0; i <= x; i++) {
        int num = i;

        while (num > 0) {
          if (num % 10 == 3) {
            count++;
          }
          num /= 10;
        }
      }
      return count;
  }

  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int T = sc.nextInt();

    for(int t = 0; t < T; t++) {
      int N = sc.nextInt();

      System.out.println(countOf3(N));
    }
    sc.close();
  }
}

```