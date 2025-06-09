# Solution

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {

	public static ArrayList<Integer> findSubmatrixSum(ArrayList<ArrayList<Integer>> arr, ArrayList<ArrayList<Integer>> queries) {

		int n = arr.size();
		int m = arr.get(0).size();

		int[][] prefix = new int[n][m];

		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				int current = arr.get(i).get(j);
				int top = i > 0 ? prefix[i-1][j] : 0;
				int left = j > 0 ? prefix[i][j-1] : 0;
				int topLeft = (i > 0 && j > 0) ? prefix[i-1][j-1] : 0;

				prefix[i][j] = current + top + left - topLeft;
			}
		}

		ArrayList<Integer> result = new ArrayList<>();
		
		for (ArrayList<Integer> q : queries) {
			int r1 = q.get(0), c1 = q.get(1), r2 = q.get(2), c2 = q.get(3);

			int total = prefix[r2][c2];
			if (r1 > 0) total -= prefix[r1 -1][c2];
			if (c1 > 0) total -= prefix[r2][c1 - 1];
			if (r1 > 0 && c1 > 0) total += prefix[r1 -1][c1 - 1];

			result.add(total);
		}
		return result;
	}
}

```