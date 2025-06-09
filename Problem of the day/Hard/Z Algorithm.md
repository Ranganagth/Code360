# Solution

```java
public class Solution {

	public static int zAlgorithm(String s, String p, int n, int m) {
		String combined = p + "$" + s;
		int len = combined.length();
		int[] z = new int[len];
		int left = 0, right = 0;

		for (int i = 1; i < len; i++) {
			if (i <= right)
			z[i] = Math.min(right - i + 1, z[i - left]);

			while (i + z[i] < len && combined.charAt(z[i]) == combined.charAt(i + z[i]))
			z[i]++;

			if (i + z[i] - 1 > right) {
				left = i;
				right = i + z[i] - 1;
			}
		}

		int count = 0;
		for (int i = m + 1; i < len; i++) {
			if (z[i] == m) {
				count++;
			}
		}

		return count;

	}

}

```