# Solution (7/10 test cases)

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
	public static long maximumSum(ArrayList<Integer> nums, int k) {
		int n = nums.size();

		long[][] dp = new long[n][k + 1];

		for (int i = 0; i < n; i++) {
			Arrays.fill(dp[i], Long.MIN_VALUE);
		}

		for (int i = 0; i < n; i++) {
			dp[i][1] = nums.get(i);
		}

		for (int j = 2; j <= k; j++) {
			for (int i = 0; i < n; i++) {
				for (int prev_i = 0; prev_i < i; prev_i++) {
					if (nums.get(i) >= nums.get(prev_i) && dp[prev_i][j - 1] != Long.MIN_VALUE) {
						dp[i][j] = Math.max(dp[i][j], (long)nums.get(i) + dp[prev_i][j - 1]);
					}
				}
			}
		}

		long maxSum = Long.MIN_VALUE;

		for (int i = 0; i < n; i++) {
			maxSum = Math.max(maxSum, dp[i][k]);
		}

		if (maxSum == Long.MIN_VALUE) {
			return -1;
		} else {
			return maxSum;
		}
	}
}

```