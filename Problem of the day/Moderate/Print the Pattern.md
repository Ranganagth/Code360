# Solution

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	public static String[] NumberPattern(int n) {

		int[][] mat = new int[n][n];
		int value = 1;

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				mat[i][j] = value++;
			}
		}

		String[] result = new String[n];
		int index = 0;

		for (int i = 0; i < n; i += 2) {
			StringBuilder sb = new StringBuilder();
			for (int val : mat[i]) {
				sb.append(val).append(" ");
			}
			result[index++] = sb.toString().trim();
		}

		for (int i = (n % 2 == 0 ? n - 1 : n - 2); i >= 0; i-= 2) {
			StringBuilder sb = new StringBuilder();
			for (int val : mat[i]) {
				sb.append(val).append(" ");
			}
			result[index++] = sb.toString().trim();
		}

		return result;
	}

}

```