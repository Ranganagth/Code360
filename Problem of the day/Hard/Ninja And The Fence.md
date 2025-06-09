# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	public static int numberOfWays(int n, int k) {
		long MOD = 1_000_000_007;

		if (n == 0) {
			return 0;
		}
		if (n == 1) {
			return (int) k;
		}

		long same = k;
		long diff = (long) k * (k - 1) % MOD;

		long total = (same + diff) % MOD;

		for (int i = 3; i <= n; i++) {
			long newSame = diff;

			long newDiff = (total * (k - 1)) % MOD;

			same = newSame;
			diff = newDiff;
			total = (same + diff) % MOD;
		}

		return (int) total;
	}
}

```