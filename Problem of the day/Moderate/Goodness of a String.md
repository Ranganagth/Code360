# Solution

```java
import java.util.* ;
import java.io.*; 
public class Solution {
	private static final int MOD = 1000000007;
	public static int goodnessOfString(String s) {
		int depth = 0;
		long goodness = 0;
		int n = s.length();

		for (int i = 0; i < n; i++) {
			char ch = s.charAt(i);

			if(ch == '[') {
				depth++;
			} else if (ch == ']') {
				depth--;
			} else if (Character.isDigit(ch)) {
				long num = 0;
				while (i < n && Character.isDigit(s.charAt(i))) {
					num = (num * 10 + (s.charAt(i) - '0')) % MOD;
					i++; 
				}
				i--;

				goodness = (goodness + (num * depth) % MOD) % MOD;
			}
		}
		return (int) goodness;
	}
}

```