# Intuition

Minimum deletions to make a string palindrome determines feasibility.

If minimum deletions ≤ K → K-Palindrome.

Key relation:
$$
\text{min deletions} = n - \text{LPS}
$$
Where LPS = Longest Palindromic Subsequence.

Thus:
$$
n - \text{LPS} \le K$$

---

# Approach

1. Compute LPS:

   * LPS = LCS(str, reverse(str))
2. Calculate:

   * deletions = n - LPS
3. Return:

   * true if deletions ≤ K else false

Use DP for LCS.

---

# Complexity

* **Time complexity:**
  $$O(n^2)$$

* **Space complexity:**
  $$O(n^2)$$

---

# Code

```Java
public class Solution {
	public static boolean isPalindrome(int k, String str) {

		int n = str.length();
		String rev = new StringBuilder(str).reverse().toString();

		int[][] dp = new int[n + 1][n + 1];

		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {

				if (str.charAt(i - 1) == rev.charAt(j - 1)) {
					dp[i][j] = 1 + dp[i - 1][j - 1];
				} else {
					dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
				}
			}
		}

		int lps = dp[n][n];
		int deletions = n - lps;

		return deletions <= k;
	}
}
```

---

# Example Walkthrough

## Example 1: Step by step explanation

str = "abba", k = 1

Reverse = "abba"

LPS = 4

Deletions = 4 - 4 = 0 ≤ 1

Answer = True

---

## Example 2:

str = "abc", k = 1

Reverse = "cba"

LPS = 1

Deletions = 3 - 1 = 2 > 1

Answer = False
