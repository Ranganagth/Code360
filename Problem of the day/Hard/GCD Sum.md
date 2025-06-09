# Solution

```java
public class Solution {
	public static int gcd(int a, int b) {
		if (b == 0) return a;
		return gcd(b, a % b);
	}

	public static long gcdSum(int n) {
		long sum = 0;
		for (int i = 1; i < n; i++) {
			for (int j = i + 1; j <= n; j++) {
				sum += gcd(i, j);
			}
		} 

		return sum;
	}

}

```