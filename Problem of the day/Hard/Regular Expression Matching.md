### Meaning of special characters

* `.` matches **any single character**.
* `*` matches \*\*zero or more of the **preceding element**.

---

# Approach: DP Table

We define:

```java
dp[i][j] = true if s[0..i-1] matches p[0..j-1]
```

Where:

* `s` is the input string
* `p` is the pattern

---

### Base cases

* `dp[0][0] = true` → Empty string matches empty pattern
* `dp[0][j]` can be `true` if `p[0..j-1]` can match an empty string (i.e., patterns like `a*`, `a*b*` etc.)

---

### Transitions

* If `p[j-1] == s[i-1]` or `p[j-1] == '.'`
  → `dp[i][j] = dp[i-1][j-1]`

* If `p[j-1] == '*'`:
  * Check zero occurrence of previous char: `dp[i][j] = dp[i][j-2]`
  * If previous char matches current `s[i-1]` (i.e., `p[j-2] == s[i-1]` or `p[j-2] == '.'`)
    → allow one more occurrence: `dp[i][j] |= dp[i-1][j]`

---

# Complexity

* **Time:** O(N*M) per test case
* **Space:** O(N*M), where `N = s.length()` and `M = p.length()`

---

# Code

```java
public class Solution {

	public static Boolean isMatch(String s, String p) {
		int n = s.length();
		int m = p.length();

		boolean[][] dp = new boolean[n + 1][m + 1];
		dp[0][0] = true;

		// Initialize dp[0][j]
		for (int j = 2; j <= m; j++) {
			if (p.charAt(j - 1) == '*') {
				dp[0][j] = dp[0][j - 2];
			}
		}

		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= m; j++) {
				if (p.charAt(j - 1) == s.charAt(i - 1) || p.charAt(j - 1) == '.') {
					dp[i][j] = dp[i - 1][j - 1];
				} else if (p.charAt(j - 1) == '*') {
					// Zero occurrences
					dp[i][j] = dp[i][j - 2];

					// One or more occurrences
					if (p.charAt(j - 2) == s.charAt(i - 1) || p.charAt(j - 2) == '.') {
						dp[i][j] = dp[i][j] || dp[i - 1][j];
					}
				}
			}
		}

		return dp[n][m];
	}
}
```

---

### Sample Driver Code (Optional for understanding and testing multiple cases)

```java
import java.util.*;

class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int T = Integer.parseInt(sc.nextLine());

		while (T-- > 0) {
			String[] parts = sc.nextLine().split(" ");
			String s = parts[0];
			String p = parts[1];
			System.out.println(Solution.isMatch(s, p));
		}
	}
}
```

---

### Example

**Input:**

```
2
aa a*
mississippi mis*is*p*
```

**Output:**

```
true
false
```

