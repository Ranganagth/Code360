# Intuition:
Given:
- A string `s`
- A dictionary of valid words

You must:
- Partition `s` into substrings such that **every substring is in the dictionary**
- Return the **minimum number of breaks** required to achieve this
- Return `-1` if it's impossible

# Approach:
We’ll use **DP**:
- Let `dp[i]` represent the **minimum number of breaks** required to segment the substring `s[0...i-1]`
- Initialize `dp[0] = 0` (no characters, no breaks)
- At each index `i`, we check all possible substrings `s[j...i-1]`. If `s[j...i-1]` is in the dictionary, update `dp[i] = min(dp[i], dp[j] + 1)`

Finally:
- If the last `dp[n] == Integer.MAX_VALUE`, return `-1`
- Otherwise return `dp[n] - 1` (we subtract 1 because if you have 1 word, it needs 0 breaks)

---

# Code: Java

```java
import java.util.*;

public class Solution {
    public static int stringBreaker(String s, int n, String[] dictionary) {
        Set<String> dict = new HashSet<>(Arrays.asList(dictionary));
        int len = s.length();
        int[] dp = new int[len + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;

        for (int i = 1; i <= len; i++) {
            for (int j = 0; j < i; j++) {
                String sub = s.substring(j, i);
                if (dict.contains(sub) && dp[j] != Integer.MAX_VALUE) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }

        return dp[len] == Integer.MAX_VALUE ? -1 : dp[len] - 1;
    }
}
```

# Code: JavaScript

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function minBreaksToFormWords(s, dict) {
    const n = s.length;
    const dp = new Array(n + 1).fill(Infinity);
    dp[0] = -1;

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            const word = s.substring(j, i);
            if (dict.has(word) && dp[j] !== Infinity) {
                dp[i] = Math.min(dp[i], dp[j] + 1);
            }
        }
    }

    return dp[n] === Infinity ? -1 : dp[n];
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
        const S = readLine().trim();
        const N = parseInt(readLine());
        const dict = new Set();

        for (let i = 0; i < N; i++) {
            dict.add(readLine().trim());
        }

        const result = dict.has(S) ? 0 : minBreaksToFormWords(S, dict);

        console.log(result);
    }

}

```

---
### Sample Test Case:
```java
stringBreaker("CODESTUDIO", 5, new String[]{"COD", "CODE", "ESTU", "DIO", "STUDIO"})
```
Output:
```
1
```

---

