# Intuition

A palindromic subsequence reads the same forward and backward.
If a string is reversed, any palindromic subsequence appears unchanged in both the original string and its reverse.
Therefore, the **Longest Palindromic Subsequence (LPS)** problem reduces to finding the **Longest Common Subsequence (LCS)** between the string and its reverse.

This reframes the problem into a standard dynamic programming formulation with strict optimal substructure.

---

# Approach

1. Let `s` be the given string of length `n`.
2. Create a reversed string `rev = reverse(s)`.
3. Define `dp[i][j]` as the length of the LCS between:

   * first `i` characters of `s`
   * first `j` characters of `rev`
4. Transition:

   * If `s[i-1] == rev[j-1]`
     `dp[i][j] = 1 + dp[i-1][j-1]`
   * Else
     `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
5. The value `dp[n][n]` is the length of the longest palindromic subsequence.

This guarantees correctness because:

* Order is preserved (subsequence).
* Matching with reverse enforces palindrome symmetry.
* Dynamic programming ensures minimal recomputation.

---

# Complexity

* **Time Complexity:** `O(n²)`
* **Space Complexity:** `O(n²)`
  Where `n` is the length of the string (≤ 100).

---

# Code

```java
import java.util.*;

public class Solution {
    public static int longestPalindromeSubsequence(String s) {
        int n = s.length();
        String rev = new StringBuilder(s).reverse().toString();

        int[][] dp = new int[n + 1][n + 1];

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == rev.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[n][n];
    }
};

```

---

# Example Walkthrough

**Input**

```
s = "bbabcbcab"
```

**Reverse**

```
rev = "bacbcbabb"
```

**Key Matches**

* Characters like `b`, `a`, `b`, `c`, `b`, `a`, `b` align in order.
* DP accumulates length when characters match symmetrically.

#### Result

```
Longest Palindromic Subsequence = "babcbab"
Length = 7
```

---

## Final Outcome

The algorithm deterministically computes the longest palindromic subsequence using optimal substructure and avoids brute-force exponential paths.



---


# Solution in JavaScript

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function longestPalindromicSubSequence(str) {
    const n = str.length;
    const dp = Array.from({ length: n }, () => Array(n).fill(0));

    for (let i = 0; i < n; i++) {
        dp[i][i] = 1;
    }

    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            if (str[i] === str[j]) {
                dp[i][j] = (len === 2) ? 2 : 2 + dp[i + 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[0][n - 1];
}


process.stdin.resume();
process.stdin.setEncoding('ascii');

var input_stdin = "";
var input_stdin_array = "";
var input_currentline = 0;

process.stdin.on('data', function (data) {
    input_stdin += data;
});

process.stdin.on('end', function () {
    input_stdin_array = input_stdin.split("\n");
    main();
});

function readLine() {
    return input_stdin_array[input_currentline++];
}


function main() {
    const T = parseInt(readLine());
    for (let t = 0; t < T; t++) {
        const str = readLine().trim();
        const result = longestPalindromicSubSequence(str);
        console.log(result);
    }
}

```