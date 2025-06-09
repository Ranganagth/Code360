# Solution

```java
import java.util.* ;
import java.io.*; 

class Solution {

  public static int internships(int n, int m, int[][] g) {
    int[] internship = new int[m];
    Arrays.fill(internship, -1);

    int count = 0;

    for (int applicant = 0; applicant < n; applicant++) {
      boolean[] visited = new boolean[m];
      if (dfs(applicant, g, visited, internship)) {
        count++;
      }
    }

    return count;

  }

  private static boolean dfs(int applicant, int[][] g, boolean[] visited, int[] internship) {
    int m = g[0].length;

    for (int j = 0; j < m; j++) {
      if (g[applicant][j] == 1 && !visited[j]) {
        visited[j] = true;

        if (internship[j] == -1 || dfs(internship[j], g, visited, internship)) {
          internship[j] = applicant;
          return true;
        }
      }
    }

    return false;
  }
}

```